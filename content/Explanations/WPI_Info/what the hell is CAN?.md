## The Basics

In the words of WPI: "Motor controllers come in two main flavors: [CAN](https://docs.wpilib.org/en/stable/docs/software/frc-glossary.html#term-CAN) and [PWM](https://docs.wpilib.org/en/stable/docs/software/frc-glossary.html#term-PWM). A *CAN* controller can send more detailed status information back to the roboRIO, whereas a *PWM* controller can only be set to a value." Basically, *CAN* controllers are two-way: information is sent to the controller, and the controller provides data back. Meanwhile, *PWM* controllers are one-way: information is sent to the controller. All of the code on the WPI website uses a PWM controller, including the code in the example projects. Code for CAN devices are provided by the website that produces the CAN controller you're working with.

Our robot uses [Rev SPARK MAX motor controllers](https://docs.revrobotics.com/sparkmax/).  Rev provides a [**list of projects**](https://opensource.revrobotics.com/#frcwpilib-example-code) showing implementation of stuff like SPARK MAX motor controllers and the Power Distribution Hub[^1]. **To use this code, you need to install Rev's vendor dependency**, which allows you to [[importing-and-exporting|import]] the `com.revrobotics` package into your code and use objects like `CANSparkMax`. [See here](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html) for info about vendor dependencies and how to install them.

link to 2024 online vendor dependency install:
https://software-metadata.revrobotics.com/REVLib-2024.json

## SPARK MAX Controllers

([`CANSparkMax` docs](https://codedocs.revrobotics.com/java/com/revrobotics/cansparkmax))
Spark Max Controllers use CAN and function similar to the PWM motor controllers that utilize the built-in FRC class `PWMMotorController`. **Motor controllers** in general allow programmers to vary the amount of voltage delivered to a motor. Motor controllers are what we actually control in code, not the motors. Our code doesn't care whether we use a Neo 550 or a Neo as our motor, it just cares that it's connected to a Spark Max controller.

It's actually pretty simple to use Spark Max controllers in a normal differential drive robot, but it gets complicated for swerve drive robots. ==So absolutely use the [Spark Max Swerve Drive example project](https://github.com/REVrobotics/MAXSwerve-Java-Template/tree/main).== ^Rev-Swerve

The other [[overview#^REVLib-implementation|Spark Max example projects]] are helpful to control various Spark Max functions in code, like reading values from its encoder or getting bus measurements.

[^1]: If you're wondering what the Power Distribution Hub (PDH) is: [here](https://docs.wpilib.org/en/stable/docs/software/can-devices/power-distribution-module.html)