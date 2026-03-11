# Pedro Pathing for FTC Autonomous
**A Comprehensive Guide to Building High-Performance Autonomous Routines**

## Table of Contents
1. [Introduction to Pedro Pathing](#introduction-to-pedro-pathing)
2. [Prerequisites and Setup](#prerequisites-and-setup)
3. [Core Concepts](#core-concepts)
4. [Building Your First Autonomous](#building-your-first-autonomous)
5. [Advanced Path Building](#advanced-path-building)
6. [State Machine Architecture](#state-machine-architecture)
7. [Integrating with Robot Subsystems](#integrating-with-robot-subsystems)
8. [Tuning and Optimization](#tuning-and-optimization)
9. [Common Patterns and Best Practices](#common-patterns-and-best-practices)
10. [Troubleshooting](#troubleshooting)
11. [Additional Resources](#additional-resources)

---

## Introduction to Pedro Pathing

IGNORE WHAT THIS SAYS BELOW IF YOU ARE JUST STARTING OUT. It is just a program that makes making autons easier.

Pedro Pathing is an advanced Reactive Vector Follower developed by FTC Team 10158 (Scott's Bots) that revolutionizes autonomous navigation in robotics. Unlike traditional pathing systems like RoadRunner, Pedro Pathing leverages Bézier curve generation to produce smoother, faster, and more efficient trajectories.

### Why Use Pedro Pathing?

- **Faster Path Execution:** Uses four vectors to calculate optimal wheel powers for quick, smooth movement
- **Dynamic Error Correction:** Continuously corrects for disturbances and gets back on path
- **On-the-Fly Path Creation:** Can instantly navigate to any position using PID control
- **Smoother Transitions:** Bézier curves ensure less jerky motions for precision tasks
- **Real-Time Adaptation:** Reacts dynamically to environmental changes

### Key Features

- **PIDF Control:** Proportional, Integral, Derivative, and Feed-Forward control for accurate motor control
- **Centripetal Force Correction:** Compensates for the robot's tendency to swing wide on curves
- **Bézier Curves:** Creates smooth, natural paths between waypoints
- **PathChains:** Allows holding end positions for actions like scoring

---

## Prerequisites and Setup

### Robot Requirements

Before using Pedro Pathing, ensure your robot meets these requirements:

- **Omnidirectional Drive:** Your robot must have mecanum, X-drive, or swerve drive (not tank drive)
- **Localization:** Must have some form of position tracking (dead wheels, Pinpoint, OTOS, or drive encoders)
- **Android Studio:** Your computer must use andriod studio, not onbot java.

### Installation

Add Pedro Pathing to your project by adding this dependency to your `build.gradle`:

```gradle
implementation 'com.github.Pedro-Pathing:PedroPathing:1.1.0'
```

Then sync your Gradle files. You can also fork the official quickstart repository from:  
[https://github.com/Pedro-Pathing/Quickstart](https://github.com/Pedro-Pathing/Quickstart)

---

## Core Concepts

### Coordinate System

Pedro Pathing uses a coordinate system where:

- Origin (0, 0) is at the bottom-left corner of the field
- Both X and Y axes span from 0 to 144 inches
- Heading is measured in radians (0 = facing right, π/2 = facing up)
- To convert from RoadRunner: add 72 to both X and Y coordinates

### Poses

A Pose represents a position and orientation on the field with three components:

```java
Pose pose = new Pose(x, y, heading);
```

Example:

```java
private final Pose startPose = new Pose(28.5, 128, Math.toRadians(180));
```

### Paths vs PathChains

**Path:** A single segment from point A to point B. The robot will not hold the end position.

**PathChain:** Multiple connected paths. Can hold the end position using the `holdEnd` parameter, useful for performing actions like scoring.

### The Follower

The Follower is the core object that manages path following. It handles:

- Tracking the robot's position via localization
- Calculating motor powers to follow the path
- Correcting for errors and disturbances

---

## Building Your First Autonomous

### Basic OpMode Structure

Here's the essential structure of a Pedro Pathing autonomous OpMode:

```java
@Autonomous(name = "Example Auto", group = "Examples")
public class ExampleAuto extends OpMode {
    private Follower follower;
    private Timer pathTimer, opmodeTimer;
    private int pathState;
    
    // Define poses
    private final Pose startPose = new Pose(28.5, 128, Math.toRadians(180));
    private final Pose scorePose = new Pose(60, 85, Math.toRadians(135));
    
    // Define paths
    private Path scorePreload;
    
    @Override
    public void init() {
        pathTimer = new Timer();
        opmodeTimer = new Timer();
        
        // Create follower
        follower = new Follower(hardwareMap);
        follower.setStartingPose(startPose);
        
        // Build paths
        buildPaths();
    }
    
    @Override
    public void loop() {
        follower.update();
        autonomousPathUpdate();
        
        // Telemetry
        telemetry.addData("path state", pathState);
        telemetry.addData("x", follower.getPose().getX());
        telemetry.addData("y", follower.getPose().getY());
        telemetry.update();
    }
}
```

### Building Paths

Create a `buildPaths()` method to define your paths:

```java
public void buildPaths() {
    // Simple straight line path
    scorePreload = new Path(new BezierLine(
        new Point(startPose),
        new Point(scorePose)
    ));
    scorePreload.setLinearHeadingInterpolation(
        startPose.getHeading(),
        scorePose.getHeading()
    );
}
```

---

## Advanced Path Building

### Using PathBuilder

The PathBuilder provides a fluent API for creating complex paths:

```java
PathChain complexPath = follower.pathBuilder()
    .addPath(new BezierLine(new Point(startPose), new Point(scorePose)))
    .setLinearHeadingInterpolation(startPose.getHeading(), scorePose.getHeading())
    .addPath(new BezierCurve(
        new Point(scorePose),
        new Point(60, 100),  // Control point
        new Point(pickup1Pose)
    ))
    .setConstantHeadingInterpolation(Math.toRadians(0))
    .build();
```

### Heading Interpolation Methods

- **`setLinearHeadingInterpolation(start, end)`:** Smoothly rotates from start to end heading
- **`setConstantHeadingInterpolation(heading)`:** Maintains a fixed heading throughout the path
- **`setTangentialHeadingInterpolation()`:** Robot faces tangent to the path (forward along the curve)

### Bézier Curves

Bézier curves create smooth, curved paths:

```java
// 2-point curve (straight line)
new BezierLine(new Point(start), new Point(end))

// 3-point curve (smooth arc)
new BezierCurve(
    new Point(start),
    new Point(controlPoint),  // Defines the curve
    new Point(end)
)

// 4-point curve (complex S-curve)
new BezierCurve(
    new Point(start),
    new Point(control1),
    new Point(control2),
    new Point(end)
)
```

### Path Constraints

Control path behavior with constraints:

```java
path.setConstantHeadingInterpolation(Math.toRadians(0));
path.setPathEndTimeoutConstraint(500);  // ms
path.setZeroPowerAccelerationMultiplier(4);
path.setReversed(true);  // Drive backwards
```

---

## State Machine Architecture

The state machine manages the sequence of actions in your autonomous. Each state represents a specific action or path segment.

### Basic State Machine Pattern

```java
public void setPathState(int pState) {
    pathState = pState;
    pathTimer.resetTimer();
}

public void autonomousPathUpdate() {
    switch (pathState) {
        case 0:
            // Start following path, set holdEnd to true for PathChains
            follower.followPath(scorePreload, true);
            setPathState(1);
            break;
            
        case 1:
            // Wait for path to complete
            if (!follower.isBusy()) {
                // Perform action (e.g., score game piece)
                outtakeClaw.open();
                setPathState(2);
            }
            break;
            
        case 2:
            // Time-based wait
            if (pathTimer.getElapsedTimeSeconds() > 1.0) {
                follower.followPath(nextPath);
                setPathState(3);
            }
            break;
            
        case 3:
            // Position-based condition
            if (follower.getPose().getX() > 50) {
                // Robot passed X coordinate 50
                setPathState(4);
            }
            break;
    }
}
```

### Path Completion Detection

Multiple ways to detect when a path is complete:

- **`follower.isBusy()`:** Returns false when robot is within 1 inch of target
- **`follower.atParametricEnd()`:** Checks if t-value reached end constraint
- **`follower.atPose(pose, xTol, yTol, headingTol)`:** Checks if at specific pose within tolerances
- **`follower.getCurrentTValue()`:** Gets parametric position along path (0.0 to 1.0)

---

## Integrating with Robot Subsystems

Pedro Pathing works seamlessly with your robot's subsystems. Here's how to coordinate path following with actions:

### Sequential Actions

```java
case 0:
    // Drive to scoring position
    follower.followPath(scorePreload, true);
    setPathState(1);
    break;
    
case 1:
    // Wait to reach position
    if (!follower.isBusy()) {
        // Raise arm while at position
        arm.setTargetPosition(HIGH_POSITION);
        setPathState(2);
    }
    break;
    
case 2:
    // Wait for arm to reach position
    if (!arm.isBusy()) {
        // Open claw to score
        claw.open();
        setPathState(3);
    }
    break;
```

### Parallel Actions

Execute subsystem actions while moving:

```java
case 0:
    // Start path
    follower.followPath(pickupPath, true);
    // Start lowering intake while moving
    intake.lower();
    setPathState(1);
    break;
    
case 1:
    // Path and intake operate simultaneously
    if (!follower.isBusy()) {
        // Both completed
        intake.grab();
        setPathState(2);
    }
    break;
```

### Using holdEnd for Actions

The `holdEnd` parameter in `followPath()` is crucial for PathChains:

```java
// holdEnd = true: Robot holds position at end of PathChain
follower.followPath(scorePathChain, true);

// holdEnd = false: Robot continues through without stopping
follower.followPath(scorePathChain, false);
```

Use `holdEnd=true` when you need to perform an action at the end position (scoring, picking up, etc.).

---

## Tuning and Optimization

Proper tuning is essential for optimal performance. Pedro Pathing requires tuning of both localization and path following parameters.

**Important Note:** The BaconBots robot uses drive motor encoders for localization (not dead wheels or external odometry pods). This affects the tuning process and constants.

### Tuning Resources

For detailed tuning instructions specific to your robot configuration, refer to:

- **Official Pedro Pathing Documentation:** [https://pedropathing.com/docs/pathing/tuning](https://pedropathing.com/docs/pathing/tuning)
- **BaconBots Team Code:** Review the team's tuned constants and configuration for a working example with drive encoders

### Constants Structure

Pedro Pathing uses two main constant classes that need to be tuned for your specific robot:

```java
// FollowerConstants - Path following behavior
public class FollowerConstants {
    // PIDF coefficients for translational and rotational control
    // See official docs and BaconBots code for tuning process
}

// LocalizationConstants - Position tracking with drive encoders
public class LocalizationConstants {
    // Encoder tick conversions and positioning
    // Critical for drive encoder-based localization
}
```

### Drive Encoder Localization

When using drive motor encoders (as opposed to dead wheels), keep in mind:

- Wheel slippage can affect accuracy, especially during rapid acceleration or on slippery surfaces
- Encoder positions are at the wheel centers, not at separate odometry pod locations
- Tuning must account for the drive train geometry and wheel diameter
- Smoother acceleration profiles can improve localization accuracy

### Tuning Workflow

Follow the official Pedro Pathing tuning guide at [https://pedropathing.com/docs/pathing/tuning](https://pedropathing.com/docs/pathing/tuning), which provides:

- Step-by-step tuning instructions
- Automated tuning OpModes
- Parameter explanations and recommended ranges
- Drive encoder-specific guidance

Reference the BaconBots code for proven constants that work with a similar drive encoder configuration.

---

## Common Patterns and Best Practices

### Cycling Pattern

For repeatedly scoring multiple game pieces:

```java
private int cycleNumber = 0;

case SCORE_SPECIMEN:
    if (!follower.isBusy()) {
        // Score specimen
        arm.score();
        claw.release();
        cycleNumber++;
        
        if (cycleNumber < 3) {
            // Continue cycling
            setPathState(DRIVE_TO_PICKUP);
        } else {
            // Done cycling, park
            setPathState(PARK);
        }
    }
    break;
```

### Emergency Stop Pattern

```java
@Override
public void loop() {
    // Check for emergency conditions
    if (opmodeTimer.getElapsedTimeSeconds() > 28.0) {
        // Emergency park with 2 seconds left
        follower.followPath(emergencyParkPath);
        pathState = -1;  // Special state to skip normal logic
    }
    
    if (pathState >= 0) {
        follower.update();
        autonomousPathUpdate();
    }
}
```

### Error Recovery Pattern

```java
case PICKUP_SAMPLE:
    if (!follower.isBusy()) {
        if (intake.hasSample()) {
            // Success
            setPathState(DRIVE_TO_BASKET);
        } else {
            // Failed to pick up, try again
            retryCount++;
            if (retryCount < 2) {
                follower.followPath(pickupPath);
                setPathState(PICKUP_SAMPLE);
            } else {
                // Give up, continue
                setPathState(PARK);
            }
        }
    }
    break;
```

### Best Practices

- **Always call `follower.update()` in `loop()`:** This is essential for path following to work
- **Reset timers when changing states:** Use `setPathState()` method that resets `pathTimer`
- **Test paths individually:** Create separate test OpModes for each path
- **Use telemetry extensively:** Log state, position, and sensor values for debugging
- **Build paths in `buildPaths()`:** Keep path building separate from `init()`
- **Use PathChains for multi-segment routes:** More efficient than separate paths

---

## Troubleshooting

### Robot Not Moving

- Check that `follower.update()` is called in `loop()`
- Verify `startingPose` is set correctly
- Ensure paths are built before being followed
- Check motor directions in hardware configuration

### Robot Oscillating or Overshooting

- P gains too high - reduce by 20-30%
- Add D gain to dampen oscillations
- Check that localization is accurate

### Robot Taking Wide Turns

- Increase centripetal force correction scaling
- Use tighter Bézier curves with closer control points
- Reduce maximum velocity for curves

### Path Not Completing

- Check `pathEndTimeoutConstraint` - may need to increase
- Verify `isBusy()` tolerance (default 1 inch) is appropriate
- Look for obstacles blocking the path

### Localization Drifting

- Verify encoder directions are correct
- Check that wheels aren't slipping on the field surface
- Recalibrate ticksToInches conversion factors for your specific wheels
- Ensure consistent traction across all drive wheels
- Refer to BaconBots code and official tuning guide for drive encoder-specific troubleshooting

---

## Additional Resources

- **Official Documentation:** [https://pedropathing.com/docs/pathing](https://pedropathing.com/docs/pathing)
- **GitHub Repository:** [https://github.com/Pedro-Pathing/PedroPathing](https://github.com/Pedro-Pathing/PedroPathing)
- **Quickstart Template:** [https://github.com/Pedro-Pathing/Quickstart](https://github.com/Pedro-Pathing/Quickstart)
- **Path Visualizer:** [https://visualizer.pedropathing.com/](https://visualizer.pedropathing.com/)
- **Discord Community:** [https://discord.gg/2GfC4qBP5s](https://discord.gg/2GfC4qBP5s)

---

*This documentation is based on Pedro Pathing 2.0.0 and official resources from the Pedro Pathing team (FTC 10158).*

  
  
[Home Page](https://potatzz.github.io/ms-robotics-resources.github.io/) || [Table of Contents](https://potatzz.github.io/ms-robotics-resources.github.io/table_of_contents.html)
