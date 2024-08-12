The following is from the `drive()` function in SwerveSubsystem of the SwerveControllerCommand project (see [here](https://github.com/wpilibsuite/allwpilib/blob/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/swervecontrollercommand/subsystems/DriveSubsystem.java)):
```java
  public void drive(double xSpeed, double ySpeed, double rot, boolean fieldRelative) {
  
  /// the code below uses a drive kinematics object (kDriveKinematics) to convert a ChassisSpeeds object to swerveModuleStates
  /// in constructing a ChassisSpeeds, you have to know whether the rotation is relative to the field or relative to the robot
  /// thus, if the field relative boolean is TRUE, we use convert the field-relative params of ChassisSpeeds to a robot-relative ChassisSpeeds object
  // if it is FALSE, we just construct a ChassisSpeeds regularly
    var swerveModuleStates =
        DriveConstants.kDriveKinematics.toSwerveModuleStates(
            ChassisSpeeds.discretize(
                fieldRelative ? // If TRUE:
	                ChassisSpeeds.fromFieldRelativeSpeeds(
                        xSpeed, ySpeed, rot, m_gyro.getRotation2d())
                    : new ChassisSpeeds(xSpeed, ySpeed, rot), // <- If FALSE
                DriveConstants.kDrivePeriod));
		...cont.
```