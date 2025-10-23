# Making Your First TeleOp Program
![Page in Progress](images/PageCaution.png)

This page will walk you through making your first TeleOp program in Andriod Studio. Make sure you have Road-Runner and Andriod studio installed, if you don't you can go [here](https://potatzz.github.io/ms-robotics-resources.github.io/code_setup.html) to install them.

# Some Terminology and Structure First
Before we begin, let's go over some basic terminology, I highly suggest you also look at the (code library) as well.
The robot uses two pieces of hardware to interpret and run the code:
- **The Driver Station** which allows the driver to run and get values from your code.
- And, **The Control Hub** which actually outputs the code and runs the motors


When writing your code, there are a few methods that are required for the robot to run. 
The first required method is `init()`, which runs **once** when you pres "Init" on the Driver Station.
The second required method is `loop()`, which runs 50x a second.
There are 3 other methods that are optional, `init_loop()`,`start()` and `stop()`.
`init_loop()` runs 50x a second after you press "Init" and before you press start, which gets added as another button on the Driver Station.
`start()` runs once after you press start, and `stop()` runs once after you press stop.
\
Our code will only use the `init()` and `loop()` methods for now.

# Start of your Program
To start your program, first create a new file Java Class in the `teamcode` folder. Name it **"MyFirstTeleOpProgram"**. Everything should start properly setup and your file should look something like this:\
```java
package org.firstinspires.ftc.teamcode.IntoTheDeep.TeleOp;

public class MyFirstTeleOpProgram {
  
}
```
This program will be moving one motor. To start, create a new variable with the type `DcMotorEx`, name it `motor`. You should get an error and `DcMotorEx` should be highlighted red. It might also suggest that you import a class. This is normal. If it suggests to import a class, import it!. A new line of code should apear at the top of the file that says `import com.qualcomm.robotcore.hardware.DcMotorEx;`, thats importing a new type of variable called DcMotorEx, which is good! If you didn't get the suggestion to import a new class, just copy and paste `import com.qualcomm.robotcore.hardware.DcMotorEx;` into the code, you can also hover over the red highlighted text to see what the problem is. 


# Using `init()`
Next, we are going to add one of our required methods, `init()`! Inside of our class `MyFirstTeleOpProgram`, create a new method with the header `public void init() {`. Inside, we're going to allow the Driver Station to locate a specific motor from the control hub!\
Add the line: `motor = hardwareMap.get(DcMotorEx.class,"motor");`. This line sets the `motor` variable we established earlier to a specific motor name and type. Although, the only part we really care about is the `"testMotor"` section at the end. In the Driver Station, you'll assign a port on the Control Hub to a specific name. This name should be the same name you put here, `testMotor`. This lets the Control Hub and Driver Station know which motor you want to move and in which port it will be plugged into.\
\
This line should give you another error, this time on the `hardwareMap.get`. It will also ask you to import something which you should do! If it doesn't ask, hover over with your mouse and import it that way.\
At this point your code should look something like this:
```java
package org.firstinspires.ftc.teamcode.IntoTheDeep.TeleOp;

import static org.firstinspires.ftc.robotcore.external.BlocksOpModeCompanion.hardwareMap;

import com.qualcomm.robotcore.hardware.DcMotorEx;

public class MyFirstTeleOpProgram {
    DcMotorEx motor;

    public void init() {
        motor = hardwareMap.get(DcMotorEx.class, "testMotor");
    }

}

```


# Movement!
Finally, let's get the motor to move! Now we will use the `loop()` method! Make a new method with the header `public void loop() {`\
Inside the `loop()` method, we're going to create an **If/Else** statement that will read the gamepad joysticks to move the motor! First write `if (gamepad1.left_stick_y > 0) {` and press enter. This line is checking to see if the y value of the left joystick on the gamepad is greater than 0, in other words, are you moving the joystick up? Again, you should get an error on the `gamepad1`, so import the class!\
Now we will add code to actually move the motor! Inside the if statement, add the line `motor.setPower(1.0);`. This line is setting the power of the `motor` variable to 1. But `motor` variable has been set to equal an actual real motor! **Keep in mind, the .setPower() attribute requires a *double* to work, so make sure you provide a double and not an integer**\
You should also keep in mind that the motors have a **-1.0 to 1.0 range of power**, if you set its power to anything higher or lower than -1.0 or 1.0, then it will just set it to -1.0 or 1.0\
\
Now we have an issue though, if you ran this code and moved the joystick, the motor would never stop! This could be dangerous, especially if its on a big robot with way more than just 1 motor! Add an `else {` statement. This section will run if the above if statement isn't met (as in, if the gamepad joy stick is less than or equal to 0 since that wouldn't run the code inside the if statement). Inside the else section, add `motor.setPower(0.0);` (**Notice how even zero's have to have a ".0" in doubles!**). This line sets the motor power to 0 if the joystick isn't pushed forward.\
This should be your final code:
```java
package org.firstinspires.ftc.teamcode.IntoTheDeep.TeleOp;


import static org.firstinspires.ftc.robotcore.external.BlocksOpModeCompanion.gamepad1;
import static org.firstinspires.ftc.robotcore.external.BlocksOpModeCompanion.hardwareMap;

import com.qualcomm.robotcore.hardware.DcMotorEx;

public class MyFirstTeleOpProgram {
    DcMotorEx motor;

    public void init() {
        motor = hardwareMap.get(DcMotorEx.class, "motor");
    }

    public void loop() {
        if (gamepad1.left_stick_y > 0) {
            motor.setPower(1.0);
        } else {
            motor.setPower(0.0);
        }
    }

}
```
# Uploading and Running your Code
wip\
\
\
Now that your code works, try a few challenges!
# Challenges:
- Make the motor spin backwards
- Make the motor speed based on how far forward the joystick is pushed. (Hint: You can put variables into .setPower())
- Add a second motor!
\
\
\
[Home](https://potatzz.github.io/ms-robotics-resources.github.io/) || [Table of Contents](https://potatzz.github.io/ms-robotics-resources.github.io/table_of_contents.html)

