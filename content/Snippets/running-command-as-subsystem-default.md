The default command is what a subsystem executes when no command is running.
(In RobotContainer) Pass the command into `.setDefaultCommand()`.

Example of a `drive()` being declared as the default command of the drive subsystem instance, `m_robotDrive`:
```java
	// Configure default commands
    m_robotDrive.setDefaultCommand(
        // The left stick controls translation of the robot.
        // Turning is controlled by the X axis of the right stick.
        new RunCommand(
            () ->
                m_robotDrive.drive(
                    // Multiply by max speed to map the 
                    // joystick unitless inputs to actual units.
                    
                    // This will map the [-1, 1] 
                    // to [max speed backwards, max speed forwards],
                    // converting them to actual units.
                    
                    m_driverController.getLeftY() * DriveConstants.kMaxSpeedMetersPerSecond,
                    m_driverController.getLeftX() * DriveConstants.kMaxSpeedMetersPerSecond,
                    m_driverController.getRightX()
                        * ModuleConstants.kMaxModuleAngularSpeedRadiansPerSecond,
                    false),
            m_robotDrive));
  }
```