To understand PID, you'll have to read WPI's documentation. They explain PID really well.
- [Control System Basics](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/control-system-basics.html)
- [Introduction to PID Article](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/introduction/introduction-to-pid.html)
- [Command-Based PID Control](https://docs.wpilib.org/en/stable/docs/software/commandbased/pid-subsystems-commands.html)

Keep in mind, you don't actually need to know any of the math. Just know how to use PID in code; you therefore could just look at a WPI example project for swerve drive and use their PID code (what we did lmao), then adjust your KPI values according to the screenshots below.

BTW, as explained in [this forum post](https://forums.firstinspires.org/forum/general-discussions/first-programs/first-robotics-competition/competition-discussion/programming-aa/java-ad/96635-difference-between-pidcontroller-and-sparkpidcontroller#post96636), it is possible to use WPI's built-in `ProfiledPIDController`/`PIDController` class with `CANSparkMax`: you don't have to use REV's provided [`SparkMaxPIDController`](https://codedocs.revrobotics.com/java/com/revrobotics/sparkmaxpidcontroller) class. That is why I didn't provide any links explaining `SparkMaxPIDController`.

Selected screenshots from the PID video:
![[PID1.png]]
(How do I set gains for P, I, and D?):
![[PID2.png]]![[PID3.png]]