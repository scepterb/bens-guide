## `Robot` class

##### `robotInit()`
Because command-based programming is a declarative paradigm, not much should be in the `robot` class. In the `robotInit()` function, an instance of `RobotContainer` should be instantiated, because that's where most of the declarative stuff is.
```java
  /**
   * This function is run when the robot is first started up and should be used for any
   * initialization code.
   */
  @Override
  public void robotInit() {
    // Instantiate our RobotContainer.  This will perform all our button bindings, and put our
    // autonomous chooser on the dashboard.
    m_robotContainer = new RobotContainer();
  }
```

##### `robotPeriodic()`
Next, in the `robotPeriodic()` function, the Command Scheduler should be run:
```java
  /**
   * This function is called every 20 ms, no matter the mode. Use this for items like diagnostics
   * that you want ran during disabled, autonomous, teleoperated and test.
   *
   * This runs after the mode specific periodic functions, but before LiveWindow and
   * SmartDashboard integrated updating.
   */
  @Override
  public void robotPeriodic() {
    // Runs the Scheduler.  This is responsible for polling buttons, adding newly-scheduled
    // commands, running already-scheduled commands, removing finished or interrupted commands,
    // and running subsystem periodic() methods.  This must be called from the robot's periodic
    // block in order for anything in the Command-based framework to work.
    CommandScheduler.getInstance().run();
  }
```

##### `autonomousInit()`
^autonomous-init

We put the logic for declaring autonomous commands in `autonomousInit()`:
```java
  /** This autonomous runs the autonomous command selected by your {@link RobotContainer} class. */
  @Override
  public void autonomousInit() {
    m_autonomousCommand = m_robotContainer.getAutonomousCommand();

    /*
     * String autoSelected = SmartDashboard.getString("Auto Selector",
     * "Default"); switch(autoSelected) { case "My Auto": autonomousCommand
     * = new MyAutoCommand(); break; case "Default Auto": default:
     * autonomousCommand = new ExampleCommand(); break; }
     */

    // schedule the autonomous command (example)
    if (m_autonomousCommand != null) {
      m_autonomousCommand.schedule();
    }
  }
```
See [[running-command-in-auto]]  for info on `.getAutonomousCommand()`
##### `teleopInit()`
It's good practice to cancel any still-running commands in `teleopInit()` :
```java
  @Override
  public void teleopInit() {
    // This makes sure that the autonomous stops running when
    // teleop starts running. If you want the autonomous to
    // continue until interrupted by another command, remove
    // this line or comment it out.
    if (m_autonomousCommand != null) {
      m_autonomousCommand.cancel();
    }
  }

```

---
## `RobotContainer` class

This class is where most of the setup for your command-based robot will take place. In this class, you will define your robot’s subsystems and commands and specify which command will run in autonomous via `getAutonomousCommand()` (see [[running-command-in-auto]]).

In the constructor you should bind those commands to triggering events with `configureBindings()`, as well as specifying which command will run in subsystem by default with `m_Subsystem.setDefaultCommand()`.

[More on this class](https://docs.wpilib.org/en/stable/docs/software/commandbased/structuring-command-based-project.html#robotcontainer)

---
## `Constants` class

Declare system-wide constants, such as ports or max speeds.

---
## `Commands` & `Subsystems` folders

See [[command-based-structure#Key Abstractions]]