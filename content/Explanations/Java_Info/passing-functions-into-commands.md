[WPI Explanation](https://docs.wpilib.org/en/stable/docs/software/basic-programming/functions-as-data.html#treating-functions-as-data-in-java) 
We often need to treat functions as data (like a variable) in programming in FRC. For example, if WPILib provides a method that is called when a button is pressed, we *pass a function* into that method that it executes when the method is activated (i.e. when the button is pressed).

In **Command-Based Programming**, we build `Command` objects that refer to a method defined in a `Subsystem` class. Many of the included `Command` types (such as `InstantCommand` and `RunCommand`) work with _any_ function - not just functions associated with a single `Subsystem`. To support building commands generically, we need to support passing functions from a `Subsystem` (which interacts with the hardware) to a `Command` (which interacts with the scheduler).

For example, the included command `Commands.runOnce()` has this code signature:
```java
public static Command runOnce(Runnable action, Subsystem... requirements)
```
We need to give this command ==a `Runnable` parameter== — a function that takes no input and provides no output. There are **two ways** to do this.

---
### Method References

To pass a pre-existing function into the command, we use *method references*.
`object::method`

```java
// Create an InstantCommand that runs the `resetEncoders` method of the `drivetrain` object
Command disableCommand = runOnce(drivetrain::resetEncoders, drivetrain);
```

---
### Lambda Expressions

To define a function inline, we use *lambda expressions*.
`() -> {function syntax;}`

```java
// Create an InstantCommand that runs the drive forward at half speed
Command driveHalfSpeed = runOnce(() -> { drivetrain.arcadeDrive(0.5, 0.0); }, drivetrain);
```