In Mr. Matheous's SwerveSubsystem code, he wrote the following in the class constructor:

```java
new Thread(() -> {
	try {
	    Thread.sleep(1000);
	        zeroHeading();
        } catch (Exception e) {}
        }).start();
```

**Threads** are a built-in Java class that can be instantiated. When it's start() method is called (This code calls the thread immediately, the thread is run CONCURRENTLY with the main() program. This way multiple tasks can be run at the same time. It's possible a thread may fail to handle its task, which is why you ALWAYS must use this try-catch structure[^1]. This statement will handle the errors that arise if your thread can't run. Threads won't work unless you add this structure.



[^1]: This structure does exactly what it looks to do: try something, catch errors if any. See [this tutorial](https://www.w3schools.com/java/java_try_catch.asp) if you're curious.