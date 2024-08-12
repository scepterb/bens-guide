---
title: Overview
---
(Here are some quick links you'll want to revisit later, conveniently located at the top of the page)
##### Important WPI Example Projects
-  [Code Template for Our Robot](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/templates/commandbased) 
- [Swerve Controller Command Example Project](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/swervecontrollercommand)
- (See full list [here](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj))
### Brief Hardware Overview

It's useful to understand how the robot's actual physical structure is set up. The important component to note is the RoboRIO, which we deploy our code to. But it'd be good to read up on what the other pieces do ([see here](https://docs.wpilib.org/en/stable/docs/controls-overviews/control-system-hardware.html)).
Here's a map of the how all FRC robots are wired:
![[FRC-Control-System-Layout.svg]]
Turn the robot on by simply connecting the battery (the big block) to the red and black wires, then flip the breaker on (or ask the hardware guys, lol).

---
### Intro to Software

At the beginning of projects, don’t forget your *include* statements. Your code needs to reference the components of WPILib (e.g. `wpilibj.RobotBase`), the third-party libraries, or the other classes in your project (e.g. `Constants`) that are used. See [[importing-and-exporting]] for more info.

You'll see a lot of (auto-generated) files in the project relating to the software **Gradle**. Gradle is a *build automation tool*, meaning it is used to convert the Java code to binary code that can be executed by the robot. The robot runs this binary code by executing the Gradle ***wrapper***, which invokes the declared version of Gradle, downloading it if necessary. A wrapper must be generated for each project. Use WPI's "Build Robot Code" command to generate this wrapper for you. Further info can be found [here](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/deploying-robot-code.html).

Review these words if some code confuses you, it might help: **[[keyword-help]]**

---
### Code Structure

View  [[structure-of-robot-code]] to learn how the code in `java/frc/robot` (where we write our code) is laid out.
When writing code, we follow a [[command-based-structure]], a declarative paradigm for structuring code.

---
### Movement

Robots are set up in certain ways to allow for movement, and these ways are called *drivetrains*. *Differential Drive* and *Mecanum Drive* are two examples of these. We use *Swerve Drive*, meaning we have four wheels that can individually change its angle and its speed (see [[Swerve-Drive-Example-Image]] to understand what I mean). 
This is the most complex driving scheme: note that **for each wheel**, robots using Swerve Drive need to declare 2 motors, 2 encoders, and 2 PID controllers (a mathematical operation which corrects our wheels from any error caused by external factors, read or [[overview#*Tell me more about PID controllers*|My guide to PID controllers]] or the [WPI guide to PID](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/controllers/pidcontroller.html)) 
```java
  private final CANSparkMax m_drivingSparkMax;
  private final CANSparkMax m_turningSparkMax;

  private final RelativeEncoder m_drivingEncoder;
  private final AbsoluteEncoder m_turningEncoder;

  private final SparkPIDController m_drivingPIDController;
  private final SparkPIDController m_turningPIDController;
```

In the WPI github library, check out the useful example projects built by WPI; these are listed [[overview#[WPI Example Projects](https //github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj)|at the beginning of this note]].

==However==, we use **CAN** to connect the components to the RoboRIO, not **PWM** like the example projects use. 
([[what the hell is CAN?]])

##### *Libraries*
- We use the [**`REVLib`**](https://docs.revrobotics.com/brushless/spark-max/revlib) library to control our CAN devices, specifically our SPARK MAX Motor Controllers. 
	- ([Docs](https://codedocs.revrobotics.com/java/com/revrobotics/package-summary.html)) 
	- ([Example Projects](https://docs.revrobotics.com/brushless/spark-max/revlib/code-examples)) ^REVLib-implementation
	- (Also see [[what the hell is CAN?#^Rev-Swerve|this note for an example Swerve Drive project]])
- The makers of the MK4i, the actual swerve motor we use, created the `swerve-lib` library so that we can access the CAN motors in our code. This code is no longer maintained, however, so we can use[**`YAGSL`**](https://github.com/BroncBotz3481/YAGSL/tree/main) 
	- ([Docs](https://broncbotz3481.github.io/YAGSL/swervelib/package-summary.html)) 
	- ([Example Projects](https://github.com/BroncBotz3481/YAGSL-Example/tree/main)) 
	- SORRY, ACTUALLY MR. MATHEOU SOMEHOW MADE CODE FOR THIS INSTEAD

##### *Tell me more about PID controllers*
Go to [[PID-control]]

##### *Tell me more about kinematics & odometry*
Go to [[what-is-kinematics-and-odometry]]

---
### Vision 
([WPI articles](https://docs.wpilib.org/en/stable/docs/software/vision-processing/index.html))
We want our robot to have *vision* and see the field so that it may operate during autonomous and help line things up in teleop. It's especially important for our robot to identify qr-code-like images placed throughout called *AprilTags*.

Vision takes two forms: **streaming** the camera directly to the DriverStation and **processing** the data on some processor (typically the roboRIO, but can also be the DriverStation computer or a Raspberry Pi processor) to actually use the info from the camera in your code.

See [[vision-and-AprilTags]] to learn how to do that.

Our camera is connected via ethernet, the settings are configured from http://limelight.local:5801/, the processing code runs on the roboRIO, and communications are handled through NetworkTables.
![[vision-workflows.drawio.svg]]

---
### Running Code

First, learn about the dashboards that run on the computer used to deploy code to the robot and to control the robot in the article [[WPI-application-utilities]]. We use the **SmartDashboard**, which runs through the applications **FRC Driver Station** as the driver's dashboard. Any data that just the programmers want to see should go through the AdvantageScope dashboard, but we don't really ever use that.

 [[running-code-with-the-robot]]
 [[simulating-robot-code]]
  
--- 