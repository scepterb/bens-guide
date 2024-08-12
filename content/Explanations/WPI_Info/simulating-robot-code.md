With the WPILib extension, the functionality is added to simulate robot code without having an actual robot to test your code with. Definitely read WPI's [Intro to Robot Simulation](https://docs.wpilib.org/en/stable/docs/software/wpilib-tools/robot-simulation/introduction.html)

*SmartDashboard* allows you to read data from your code:
```java
SmartDashboard.putNumber("Bus Voltage", busVoltage);
```