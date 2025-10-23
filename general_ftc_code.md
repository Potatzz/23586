# General FTC Code
This page is based off of and written from the LearnJavaForFTC pdf file in our GitHub. Go check it out for more information!\
**This page will assume you have a decent or general knowledge of how to use, made and the terminology involving: *Methods, Classes, Variables, Loops, Conditionals, and Importing things in Java***


# OpMode and LinearOpMode
When coding for FTC, there are two different "versions" of organization and running your code. Both have their advantages, but for my own convienence I will be programming using OpMode for the rest of this page. You can use which ever one you'd like too though.\

## OpMode
OpMode is a much more organized way of running through your code. It requires two methods to run: `public void init() {}` and `public void loop() {}`. \
`public void init() {}` runs once when the driver presses "INIT" on the Drivers Station. \
`public void loop() {}` runs repeatedly 50 times a second when the driver presses "PLAY" on the Drivers Station.\
\
Additionally, there are 3 other methods you can use but aren't strictly necesary for your code to run. 
`public void init_loop() {}` runs repeatedly 50 times a second when the driver presses "INIT", but **before*** the driver presses "PLAY". This is helpful for calibrating motors and servos.
`public void start() {}` runs once after the driver presses "PLAY". 
`public void stop() {}` runs once after the driver presses "STOP". This is helpful to end any processes that need to be terminated before you turn of the program. 

# LinearOpMode
LinearOpMode doesn't use multiple methods to differentiate between INIT, START, and STOP. Instead, there is one loop, `public void runOpMode() {}` and several commands like, `waitForStart()` to differentiate between INIT, START, and STOP.\
`public void runOpMode() {}` is the only method in LinearOpModes. It is required to have\
`waitForStart()` waits for the driver to press "START" before continuing execution of the code.\
`opModeIsActive()` returns true or false if the code is in its "loop" period.\
\
To create a file using LinearOpMode you would write:
```java
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
