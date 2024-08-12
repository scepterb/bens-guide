In the `Robot` class, we run getAutonomousCommand() and schedule the autonomous command it returns (see [[structure-of-robot-code#^autonomous-init]]). In `RobotContainer`, we define what getAutonomousCommand does:

```java
// Run drive() once when autonomous mode is initiated
  public Command getAutonomousCommand() {
    return new InstantCommand(() -> m_robotDrive.drive(1, 1, 0, false)));
  }
```

NOTE: To create a command that runs continuously, use `RunCommand()`