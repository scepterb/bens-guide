### `extends` 
Declares a class as a subclass of another class.

### `@override` 
`@override` is a feature that allows a subclass to provide a specific implementation of a method already provided by the parent class (allowing what's called "run-time polymorphism").
**[[overriding-keyword]] example snippet**

### `final`
The `final` keyword has different contexts depending on where it is used:
- ***Variables*** (the big one): When a variable is declared as final, its value **cannot be changed** once it has been initialized. This is useful for declaring constants or other values that should not be modified.
- ***Methods***: When a method is declared as final, it cannot be overridden by a subclass. This is useful for methods that are part of a class’s public API and should not be modified by subclasses.
- ***Classes***: When a class is declared as final, it cannot be extended by a subclass. This is useful for classes that are intended to be used as is and should not be modified or extended.
It's good to make all constants immutable, aka define them using the `final` keyword.

### `this`
See [[this-keyword]].

### `?` operator (aka "ternary operator)
A traditional if-else construct in Java is written as

```java
if (a > b) {
    result = x;
} else {
    result = y;
}
```

This can be rewritten as the following statement:

```java
result = a > b ? x : y;
```

See [[ternary-operator-example]] for an example of the operator in the WPI example swerve drive project.

### `get` & `set`
Making variables public is always dangerous because *any* other class can access that variable, even accidentally (imagine having a million public variables named `a`). That's why we use **getters and setters**, public methods whose sole purpose is to allow other classes to read and write to the variables. (So get and set aren't really keywords, but the words that all of these methods start with)
```java
public class Person {
  private String name; // private = restricted access

  // Getter
  public String getName() {
    return name;
  }

  // Setter
  public void setName(String newName) {
    this.name = newName;
  }
}
```