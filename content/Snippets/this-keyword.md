The most common reason for using the `this` keyword is because a field is shadowed by a method or constructor parameter.

```java
public class Point {
    public int x = 0;
    public int y = 0;
    
    //constructor
    public Point(int x, int y) {
        this.x = x; //sets the object's x to equal the inputted parameter
        this.y = y;
    }
}
```

```java
// identical code with no "this" keyword
public class Point {
    public int a = 0;
    public int b = 0;
    
    //constructor
    public Point(int x, int y) {
        a = x;
        b = y;
    }
}
```