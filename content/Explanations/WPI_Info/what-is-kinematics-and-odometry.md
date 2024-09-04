you're probably tired of hearing me talk about movement, but listen up, this is important:
>[!info]
> **Later note: the website for [Yet Another Generic Swerve Library](https://yagsl.gitbook.io/yagsl/fundamentals/swerve-drive) explains this 10 times better than me.**

---
The purpose of **kinematics**[^1] is to convert between a universal `ChassisSpeeds` class (view [docs](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/math/kinematics/ChassisSpeeds.html)) to multiple` SwerveModuleState` classes, which contains velocities and angles for each wheel of our drivetrain. `SwerveDriveKinematics` is the Writing to `ChassisSpeeds` will change the swerve module state and move our robot!

Want to know more about what `ChassisSpeeds` is? Here:
(From [WPI's Info about Chassis Speeds](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/intro-and-chassis-speeds.html#constructing-a-chassisspeeds-object))
![[chassis-speeds.png]]

In the swerve drive subsystem, I used `ChassisSpeeds` to create a drive function:
([more info on how the code works here](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/swerve-drive-kinematics.html))
```java
public void drive(double xSpeed, double ySpeed, double rot, boolean fieldRelative) {
	var swerveModuleStates =
		DriveConstants.kDriveKinematics.toSwerveModuleStates(
			ChassisSpeeds.discretize(
				fieldRelative
				? ChassisSpeeds.fromFieldRelativeSpeeds(
					xSpeed, ySpeed, rot, this.getRotation2d())
				: new ChassisSpeeds(xSpeed, ySpeed, rot),
			0.02)); // 0.02 = once per scheduler call
	SwerveDriveKinematics.desaturateWheelSpeeds(
		swerveModuleStates, DriveConstants.kTeleDriveMaxSpeedMetersPerSecond);
	frontLeft.setDesiredState(swerveModuleStates[0]);
	frontRight.setDesiredState(swerveModuleStates[1]);
	backLeft.setDesiredState(swerveModuleStates[2]);
	backRight.setDesiredState(swerveModuleStates[3]);
}
```

---

The purpose of **odometry** is to use the robot's sensors to estimate the *pose* of our robot. A pose is represented with the `Pose2d` class, and it consists of a *translation* and a *rotation*.

In the swerve drive subsystem, we create a `SwerveDriveOdometry` instance[^2] and update it in `periodic()` with the robot's rotation and an array of swerve module positions:
([more info on how the code works here](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/swerve-drive-odometry.html))
```java
  // Odometry class for tracking robot pose
  SwerveDriveOdometry m_odometry =
      new SwerveDriveOdometry(
          DriveConstants.kDriveKinematics,
          m_gyro.getRotation2d(),
          new SwerveModulePosition[] {
            m_frontLeft.getPosition(),
            m_frontRight.getPosition(),
            m_rearLeft.getPosition(),
            m_rearRight.getPosition()
          });

  /** Creates a new DriveSubsystem. */
  public DriveSubsystem() {}

  @Override
  public void periodic() {
    // Update the odometry in the periodic block
    m_odometry.update(
        m_gyro.getRotation2d(),
        new SwerveModulePosition[] {
          m_frontLeft.getPosition(),
          m_frontRight.getPosition(),
          m_rearLeft.getPosition(),
          m_rearRight.getPosition()
        });
  }
```



[^1]: Technically, I should be calling it **inverse kinematics**. *Kinematics* is using wheel angles to determine where our robot will go (SwerveModuleState -> ChassisSpeeds), while *inverse kinematics* is using where we want our robot to go to determine the angles and speeds of our wheels (ChassisSpeeds -> SwerveModuleState) This article titled [What is Inverse Kinematics?](https://abhinavwastaken.medium.com/using-inverse-kinematics-to-become-a-master-swerver-1026759d81b0) is a nice run-down if you're curious about inverse kinematics.

[^2]: We might switch to using a "pose estimator" instead of an odometry class after reading [this WPI article about pose estimators](https://docs.wpilib.org/en/stable/docs/software/advanced-controls/state-space/state-space-pose-estimators.html). It integrates vision into odometry to give a more accurate pose.