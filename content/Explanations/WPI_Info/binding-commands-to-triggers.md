([WPI Explanation](https://docs.wpilib.org/en/stable/docs/software/commandbased/binding-commands-to-triggers.html))
If your command isn't an **autonomous command** or a **default command**, then you need to **bind your command to a trigger** for the driver to be able to run the command.

[Trigger documentation](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/button/Trigger.html) <- Useful for viewing `Trigger` functions, like `OnTrue()` and `WhileFalse()`.

##### Declare instance of trigger, then binding command to trigger instance:
Declare trigger using CommonGenericHID factories ([docs](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/button/CommandXboxController.html)) in `configureBindings()`:
```java
CommandXboxController exampleCommandController = new CommandXboxController(1); // Creates a CommandXboxController on port 1.
Trigger xButton = exampleCommandController.x(); // Creates a new Trigger object for the `X` button on exampleCommandController
```
Then you can bind the command:
```java
xButton.onTrue(new ExampleCommand(m_exampleSubsystem))
```

##### Alternatively, declare and bind trigger instance *in-line*: 
Creates trigger in the `configureBindings()` function (in RobotContainer), then declare an `onTrue()` binding.
```java
import edu.wpi.first.wpilibj.templates.commandbased.commands.ExampleCommand;

public class RobotContainer {
	private final HatchSubsystem m_hatchSubsystem = new HatchSubsystem();
	
	private void configureBindings() {
		// Schedule `ExampleCommand` when `exampleCondition` changes to `true`
	    new Trigger(m_hatchSubsystem::exampleCondition)
			.onTrue(new ExampleCommand(m_exampleSubsystem));
  }
}
```

(In this case, `exampleCondition` is a method in `HatchSubsystem`):
```java
public boolean exampleCondition() {
	// Query some boolean state, such as a digital sensor.
	return false;
}
```

Because you can declare a command in-line AND bind it to a trigger in-line, you can do both in-line. So when the command to run is simple, you can do all that in-line. This is actually what we did in the beginning of the command-based structure note ([[command-based-structure#^in-line|here]]).

---
### More fun with triggers

**Chaining Calls:** It is useful to note that the command binding methods all return the trigger that they were called on, and thus can be chained to bind multiple commands to different states of the same trigger. For example:

```java
xButton
    // Binds a CommandA to be scheduled when the button is pressed
    .onTrue(new CommandA())
    // Binds a CommandB to be scheduled when that same button is released
    .onFalse(new CommandB());
```

**Composing Triggers:** The `Trigger` class can be composed to create composite triggers through the `and()`, `or()`, and `negate()` methods (or, in C++, the `&&`, `||`, and `!` operators). For example:
```java
// Binds an ExampleCommand to be scheduled when both the 'X' and 'Y' buttons of the driver gamepad are pressed
exampleCommandController.x()
    .and(exampleCommandController.y())
    .onTrue(new ExampleCommand());
```

**Debouncing:** aka coding for the "rising edge" — don't worry about this unless some commands are running twice
```java
// debounces exampleButton with a 0.1s debounce time, rising edges only
exampleButton.debounce(0.1).onTrue(new ExampleCommand());

// debounces exampleButton with a 0.1s debounce time, both rising and falling edges
exampleButton.debounce(0.1, Debouncer.DebounceType.kBoth).onTrue(new ExampleCommand());
```