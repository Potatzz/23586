# Field Centric Drive Code:
This page has the code for Field Centric Drive. If you don't know, FCD (Field Centric Drive), is very useful in comp.\
If you imagine driving a robot normally, as long as the robot is facing forward relative to you, it drives normally. But if you were to turn the robot, instead of going forward relative to you, it will go forward relative to the robot. This can be bad since during comp its difficult to keep track of the robot heading and everything else going on around you.\
FCD makes it so the driver has one less thing to worry about during comp and makes it easier to drive.

## The Code
To use the FCD code, make a new .java file named: `FieldCentricDriveNavX` and paste this to the file:
```java
package org.firstinspires.ftc.teamcode;


import com.qualcomm.hardware.kauailabs.NavxMicroNavigationSensor;
import com.qualcomm.robotcore.eventloop.opmode.OpMode;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.DcMotorEx;
import org.firstinspires.ftc.robotcore.external.navigation.AngleUnit;
import org.firstinspires.ftc.robotcore.external.navigation.AxesOrder;
import org.firstinspires.ftc.robotcore.external.navigation.AxesReference;

import static com.qualcomm.robotcore.hardware.DcMotorSimple.Direction.REVERSE;


@TeleOp(name="FieldCentricDriveNavX")
public class FieldCentricDriveNavX extends OpMode {
    
    // Initiates a navx variable that will recieve data from the navx (robots direction)
    NavxMicroNavigationSensor navx;

    // Motors and their names
    DcMotorEx frontLeft;
    DcMotorEx backLeft;
    DcMotorEx frontRight;
    DcMotorEx backRight;

    // Joystick variables
    double lJoyY;
    double lJoyX;
    double rJoyX;


    // Joystick direction using atan2 (no clue how it works but it does). Technically not needed but nice to have for debugging
    public static double joyDir(double joyX, double joyY) {
        return Math.atan2(joyY,joyX);
    }

    // Init method. Runs once when init is pressed
    public void init() {

        // Setting config names
        navx = hardwareMap.get(NavxMicroNavigationSensor.class,"imu");

        frontLeft = hardwareMap.get(DcMotorEx.class,"LeftFront");
        backLeft = hardwareMap.get(DcMotorEx.class, "LeftBack");
        backRight = hardwareMap.get(DcMotorEx.class,"RightBack");
        frontRight = hardwareMap.get(DcMotorEx.class,"RightFront");

        // Reversing motors so that the robot actually moves forward -_-
        frontRight.setDirection(REVERSE);
        backRight.setDirection(REVERSE);

        // This ensures that the robot doesn't drift when you stop applying power via the joysticks
        frontRight.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        frontLeft.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        backRight.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
        backLeft.setZeroPowerBehavior(DcMotor.ZeroPowerBehavior.BRAKE);
    }
    
    
    public void loop() {
        // Getting yaw value (aka the rotation of the robot). Getting it in Radians cause math is weird and doesnt like Degrees.
        double yaw = navx.getAngularOrientation(AxesReference.INTRINSIC,AxesOrder.XYZ,AngleUnit.RADIANS).thirdAngle;

        // Setting joystick variables to actually equal the joystick values.
        // We only need the x value for the right joystick since we only use it for turning
        lJoyY = gamepad1.left_stick_y;
        lJoyX = gamepad1.left_stick_x;
        rJoyX = gamepad1.right_stick_x;

        // These two variables take the joystick values and the robot direction
        // They do math to counteract the robots correct direction
        double robotRotY = lJoyX * Math.sin(yaw) + lJoyY * Math.cos(yaw);
        double robotRotX = - lJoyY * Math.sin(yaw) + lJoyX * Math.cos(yaw);

        // The above variables could technically return values above or below 1 or -1. This is bad since the motors
        // This is bad since the motors will just set the value to 1 or -1 if they're given a power value outside their range (-1 -> 1)
        // This will make sure that the same ratio of power is kept.
        //
        // Ex: the left motors are set to 20 and the right motors are set to 10 due to the math above.
        // Normally all the motors would go at full speed not turning at all.
        // But the variable below instead makes it so that the left motors go at full speed, while the right ones go at half speed
        double motorPower = Math.max(Math.abs(robotRotY) + Math.abs(robotRotX) + Math.abs(rJoyX),1);


        // Sets the motor values. Everything is divided by motorPower so that the same ratio of speed is kept
        frontLeft.setPower((robotRotY + robotRotX + rJoyX) / motorPower);
        backLeft.setPower((robotRotY - robotRotX + rJoyX) / motorPower);
        frontRight.setPower((robotRotY - robotRotX - rJoyX) / motorPower);
        backRight.setPower((robotRotY + robotRotX - rJoyX) / motorPower);

        // Telemetry data. Helpful for debugging and general information about the robot :)
        telemetry.addData("Joy Dir: ", Math.toDegrees(joyDir(lJoyY,lJoyX)));
        telemetry.addData("Robot Dir:  ",Math.toDegrees(yaw));
        telemetry.addData("Robot Dir (RADIANS): ",Math.toRadians(yaw));
        telemetry.addData("Joystick Y: ",lJoyY);
        telemetry.addData("Joystick X: ", lJoyX);
        telemetry.addData("-------------------","--------");
        telemetry.addData("Front Left: ",(robotRotY + robotRotX + rJoyX) / motorPower);
        telemetry.addData("Front Right: ",(robotRotY - robotRotX - rJoyX) / motorPower);
        telemetry.addData("Back Left: ",(robotRotY - robotRotX + rJoyX) / motorPower);
        telemetry.addData("Back Right: ",(robotRotY + robotRotX - rJoyX) / motorPower);


        telemetry.update();
    }



}
```
