# General FTC Code
This page is based off of and written from the [LearnJavaForFTC](https://github.com/Potatzz/ms-robotics-resources.github.io/tree/bbe5959fdd6d67d1f036102f8bf4f4e890d67479/Downloadable%20Resources) downloadable from [our GitHub](https://github.com/Potatzz/ms-robotics-resources.github.io)!\
### This page will assume you have a decent or general knowledge of how to use, made and the terminology involving: *Methods, Classes, Variables, Loops, Conditionals, and Importing things in Java*. It also assumes that you know what the Control Hub and Drivers Station is and that you are using Andriod Studio to program
(Both Andriod Studio and OnBotJava have their advatages and disadvatages, but many teams (including ours) mainly use Andriod Studio.)


# OpMode and LinearOpMode
When coding for FTC, there are two different "versions" of organization and running your code. Both have their advantages, but for my own convienence I will be programming using OpMode for the rest of this page. You can use which ever one you'd like too though.\

## OpMode
OpMode is a much more organized way of running through your code. It requires two methods to run: `public void init() {}` and `public void loop() {}`. \
`public void init() {}` runs once when the driver presses "INIT" on the Drivers Station. \
`public void loop() {}` runs repeatedly 50 times a second when the driver presses "PLAY" on the Drivers Station.\
\
Additionally, there are 3 other methods you can use but aren't strictly necesary for your code to run. \
`public void init_loop() {}` runs repeatedly 50 times a second when the driver presses "INIT", but **before*** the driver presses "PLAY". This is helpful for calibrating motors and servos.\
`public void start() {}` runs once after the driver presses "PLAY". \
`public void stop() {}` runs once after the driver presses "STOP". This is helpful to end any processes that need to be terminated before you turn of the program. \
\
To create a file using OpMode you would write:
```java
@TeleOp()
public class fileName extends OpMode {
  public void init() {
    // Your code goes here
  }

  public void loop() {
    // Your code goes here
  }
}
```
You may notice the `@TeleOp()`. This is actually cruicial to your code. It lets the Drivers Station figure out whether or not your program is a TeleOp or Autonomous program. In other words, a program where a human can use a gamepad and control the robot, or a program where the robot moves on its own to score.\
\
You can add in the other 3 methods in between `init()` and `loop()` where they should logically fit (Ex: `public void stop() {}` would go at the end, since it is the last thing to run when you want to end your program on the Drivers Station)

# LinearOpMode
LinearOpMode doesn't use multiple methods to differentiate between INIT, START, and STOP. Instead, there is one loop, `public void runOpMode() {}` and several commands like, `waitForStart()` to differentiate between INIT, START, and STOP.\
`public void runOpMode() {}` is the only method in LinearOpModes. It is required to have\
`waitForStart()` waits for the driver to press "START" before continuing execution of the code.\
`opModeIsActive()` returns true or false if the code is in its "loop" period.\
\
To create a file using LinearOpMode you would write:
```java
@TeleOp()
public class fileName extends LinearOpMode {
  // Variables and anything else goes up here

  public void runOpMode() {
    // Code that would normally go in init() goes here

    waitForStart();
    while (opModeIsActive()) {
      // Code that would normally go in loop() goes here
    }
  }
}
```
The downside to using LinearOpMode is that you are responsible for updating telemetry (FTC equivalent to System.out.print()) and for ensuring that loops aren't forever.


# Imports
When programming for FTC, you will have to import classes and libararies **often**. So it's good to get to know how. If you write a piece of code, for example:
```java
if (gamepad1.left_stick_y > 1.0) {
  motor.setPower(1.0);
}
```
Andriod Studio will highlight `gamepad1.left_stick_y` and throw an error. This is because, while you can use `gamepad1.left_stick_y`, you need to import the proper class. If you hover over it, it will ask if you want to import a class. If you hit import class, it should import a gamepad class above. \
If you are using OnBotJava, it should automatically import classes and libararies for you. 

# Telemetry
Telemetry is FTC's `System.out.print()` or `print()` equivalent. It is used to send human-readable data to the Driver's Station. \
Telemetry has many methods that you can use. But the most commonly used ones are `telemetry.addData()` and `telemetry.update()`.\
`telemetry.addData()` takes in two arguments, one for the caption which is like a descriptive header (ex: "x: ", "width: " or "id: "), and the other is for any data you want to output. The second argument can take anything from variable names to Strings.\
`telemetry.update()` takes in no arguments. It is used to refresh and resend data to the Drivers Station. For any telemetry data to show up, you need to run this. \
Some sample telemetry code is:
```java
int x = 400;
telemetry.addData("","hi!");
telemetry.addData("x: ",x);
telemetry.update();
```

# Gamepad Input
A key part of the robot, is being able to take in human input! The way that every team does is via a gamepad. But how do you recieve and check the input from one? You use the gamepad class. It lets you check the values of every button on the gamepad.\


# Motors


# Servos















[Home Page](https://potatzz.github.io/ms-robotics-resources.github.io/) || [Table of Contents](https://potatzz.github.io/ms-robotics-resources.github.io/table_of_contents.html)
