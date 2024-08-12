To declare any class as a package that you can import in another class, use `package`:
```java
package frc.robot;

public final class Constants {

}
```
To import that class, use `import`:
```java 
import frc.robot.Constants
```

# The `Constants` class

In this class, we declare multiple subclasses holding variables that store, you guessed it, *constants*. Examples of information that should be held in a constant include pin mappings, & wheel diameters.

The following details the subclasses that a `Constants` class should contain:
- `DriveConstants`
	- For DriveSubsystem
	- e.g. ports for motors and encoders
- `ModuleConstants`
	- For SwerveModule
- `OIConstants`
	- Stands for "Operator Interface", aka the stuff that pertains to driver input
	- e.g. driver controller port ID