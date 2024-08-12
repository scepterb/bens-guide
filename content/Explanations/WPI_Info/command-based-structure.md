## ([wpi expanation of command-based programming](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html))

“Command-Based” can refer to either the *design pattern* of the program or the *WPILib Libraries* that facilitate the structure.

```java
// command-based design pattern
new Trigger(condition::get).onTrue(Commands.runOnce(() -> piston.set(DoubleSolenoid.Value.kForward))); //lambda expression
```
^in-line

```java
// typical design pattern
if(condition.get()) {
  if(!pressed) {
    piston.set(DoubleSolenoid.Value.kForward);
    pressed = true;
  }
} else {
  pressed = false;
}
```

Note that in the first example, we use a *lambda expression* to pass the `piston.set()` as a parameter of the command `.runOnce()`. You will often need to pass functions as a parameter in command-based programming so that you may run functions using commands. See [[passing-functions-into-commands]] for more info on the code structures used to do this.
### Key Abstractions:

- **Commands** represent actions the robot can take. Commands run when scheduled, until they are interrupted or their end condition is met. [Commands can be composed together](https://docs.wpilib.org/en/stable/docs/software/commandbased/command-compositions.html) to accomplish more-complicated tasks. See [Commands](https://docs.wpilib.org/en/stable/docs/software/commandbased/commands.html#commands) for more info.
- **Subsystems** represent independently-controlled collections of robot hardware (such as motor controllers, sensors, pneumatic actuators, etc.) that operate together. Subsystems back the resource-management system of command-based: only one command can use a given subsystem at the same time.

### Command Scheduler:

Commands are run by the **CommandScheduler singleton** ([docs](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/CommandScheduler.html)), which polls triggers (such as buttons) to schedule commands, prevents resource conflicts, and executes scheduled commands. The scheduler’s *run()* method must be called from the *robotPeriodic()* method of the *Robot* class (runs once every 20ms).

<u>Multiple commands can run concurrently, as long as they do not require the same resources on the robot.</u> Resource management is handled on a per-subsystem basis: commands specify which subsystems they interact with, and the scheduler will ensure that no more more than one command requiring a given subsystem is scheduled at a time.

---
### So, how do I write a command?

Good question! You can always define a command in-line with `Commands`, as seen [[command-based-structure#^in-line|in the beginning]]. But what if you need to run a certain command in multiple situations? ^in-line-mention

First, let's walk through a WPI-provided example command, found [here](https://github.com/wpilibsuite/allwpilib/blob/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/templates/commandbased/commands/ExampleCommand.java), which is written as a separate command class by *subclassing `Command`*. Despite how intuitive this is, programmers don't often use this approach because it's very verbose. So later, we'll explore the *instance factory method*, where we define commands within a subsystem class. 
(in the above, note the difference between [`Command`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/Command.html#schedule()), a type, and [`Commands`](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/Commands.html) , which constructs a `Command`)

##### Subclassing `Command` %% fold %% 
Let's say you already wrote code for a certain subsystem — say, our robot's arm — and now you would like to write a repeatable command for that subsystem. A simple way to do this is by writing a *custom command class* by *subclassing `Command`*.

First, imports and packaging (see [[importing-and-exporting]] if you're confused)
```java
package frc.robot.commands;

import frc.robot.subsystems.HatchSubsystem; // your subsystem here
import edu.wpi.first.wpilibj2.command.Command; // the WPI Command class
```

Next, declare your command class. We're subclassing our command under the WPI `Command` class, but you won't always do this. For example, you would subclass the WPI `PIDCommand` class to easily make commands with a built-in PIDController. (Other command classes can be found [here](https://docs.wpilib.org/en/stable/docs/software/commandbased/commands.html#control-algorithm-commands))
Fundamentally, each command has [four lifecycle methods](https://docs.wpilib.org/en/stable/docs/software/commandbased/commands.html#the-structure-of-a-command): 
- `initialize()`
	- Called once when command is scheduled, should place the command in a known state for the start of execution. Any state or resources needed for the command’s functionality should be initialized or opened here and not in the constructor
- `execute()`
	- Called once each time the scheduler is run (20ms)
- `end()`
	- Called once when the command ends, either by finishing normally (`isFinished()` returns `True`) or by being interrupted. This method takes the boolean argument `interrupted` to specify how the command ended. All your code should be "wrapped up" here, returning everything to a default state.
- `isFinished()`
	- Should return `True` when the command ends to inform the scheduler
By overriding these methods, we tell the command what to do in each of its possible states. (see [[overriding-keyword]] to see how overriding works)

Finally, declare the command's constructor. In the constructor you should declare all necessary subsystem dependencies
```java
package frc.robot.commands;

import frc.robot.subsystems.HatchSubsystem;
import edu.wpi.first.wpilibj2.command.Command;

/**
 * A simple command that grabs a hatch with the {@link HatchSubsystem}. Written explicitly for
 * pedagogical purposes. Actual code should inline a command this simple with InstantCommand.
 */
public class GrabHatch extends Command {
  // The subsystem the command runs on
  private final HatchSubsystem m_hatchSubsystem;

  public GrabHatch(HatchSubsystem subsystem) {
    m_hatchSubsystem = subsystem;
    addRequirements(m_hatchSubsystem); // "dependency injection"
  }

  @Override
  public void initialize() {
    m_hatchSubsystem.grabHatch();
  }

  @Override
  public boolean isFinished() {
    return true;
  }
}
```

You would then [[binding-commands-to-triggers|bind this command to a trigger]] OR [[running-command-as-subsystem-default|run command defaultly]] OR [[running-command-in-auto|run the command in autonomous mode]]. 

##### Instance Factory Method

In the example above, we simply run the grabHatch() function. This is simple enough that it would be better to just define the command in-line, [[command-based-structure#^in-line-mention|as said before]]. But if you still want to run this command multiple times, you can ***define a function that returns a command as an object***, called the "Instance Factory Method", instead of writing a separate grabHatch() function and command. In doing this you must use [one of WPI's included command types](https://docs.wpilib.org/en/stable/docs/software/commandbased/commands.html#included-command-types).
- runOnce(), backed by InstantCommand type
- run(), backed by RunCommand type
- startEnd(), backed by StartEndCommand type
Additionally, included command types exist for waiting and for various control setups:
- waitSeconds(), backed by WaitCommand type
- [PIDCommand](https://docs.wpilib.org/en/stable/docs/software/commandbased/pid-subsystems-commands.html#pidcommand) type, [ProfiledPIDCommand](https://docs.wpilib.org/en/stable/docs/software/commandbased/profilepid-subsystems-commands.html#profiledpidcommand) type, [SwerveControllerCommand](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/SwerveControllerCommand.html) type... again, the full list can be found [here](https://docs.wpilib.org/en/stable/docs/software/commandbased/commands.html#control-algorithm-commands)

<u>My example implementation</u>
runOnce:
```java
  // Example: grabs the hatch
  public Command grabHatchCommand() {
    // implicitly require `this`
    // returns command that grabs hatch
    return this.runOnce(() -> m_hatchSolenoid.set(kForward));
  }
  
  // Also, you might find the following in your project template:
  public Command exampleMethodCommand() {
    // Inline construction of command goes here.
    // Subsystem::RunOnce implicitly requires `this` subsystem.
    return runOnce(
        () -> {
          /* one-time action goes here */
        });
  }
```
To create a command object, use InstantCommand (in-line):
```java
new InstantCommand(() -> m_robotDrive.arcadeDrive(
    -driverController.getLeftY(),
    driverController.getRightX()),
    m_robotDrive)
```


You would then [[binding-commands-to-triggers|bind this command to a trigger]] OR [[running-command-as-subsystem-default|run command defaultly]] OR [[running-command-in-auto|run the command in autonomous mode]]. 

NB: For any subsystem **requirements**, first initialize the subsystems in your subsystem constructor. Then you can add many subsystem objects as a requirement as after the lambda (the *runnable*) in an included command type:
`runOnce(Runnable, Requirements...)`

---
### Reprise of writing commands

We went over the top two approaches to defining commands, as well as defining commands in-line. See WPI's full article about theses approaches [here](https://docs.wpilib.org/en/stable/docs/software/commandbased/organizing-command-based.html#instance-command-factory-methods).
![[Clean-Code-Structures.png]]