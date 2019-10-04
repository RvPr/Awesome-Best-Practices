Avoid Nulls
===========

Guideline
---------
Avoid having methods return a `null` reference in languages like Java. Instead, return an `Optional`. Or if it represents an error-case, throw a custom-exception which can be caught if needed.

Benefits
---------
By making it very clear that the method could return an `empty` result, it forces the caller to write code that handles such cases appropriately.

Conversely, if a method does not return a `Optional`, the caller can be guaranteed that it will never return an empty result. And they don't need to write unnecessary code handling a corner case that will not happen.

References
----------
1. [StackOverflow discussion](https://stackoverflow.com/questions/23454952/uses-for-optional)
1. [Oracle.com technical writeup](https://www.oracle.com/technetwork/articles/java/java8-optional-2175753.html)
1. [The "father of nulls" calls it his biggest mistake](https://en.wikipedia.org/wiki/Tony_Hoare#Apologies_and_retractions)

Examples
---------

Bad:
```java
// Will always return the user's unique ID
public UUID getUserID() {
  return Preconditions.checkNotNull(this.id);
}

// Will return user's first name if specified. Otherwise, returns null
public String getFirstName() {
  return this.firstName;
}
```

Better:
```java
public UUID getUserID() {
  return Preconditions.checkNotNull(this.id);
}

public Optional<String> getFirstName() {
  return Optional.ofNullable(this.firstName);
}
```

-----

Bad:
```java
// Will return user's account balance. If the system is down, will return null
public Integer getAccountBalance() {
  return system.isAvailable() ? system.getBalance() : null;
}
```

Better:
```java
// Will return user's account balance. If the system is down, will throw SystemUnavailableException
public int getAccountBalance() {
  if (!system.isAvailable()) {
    throw new SystemUnavailableException();
  } else {
    return system.getBalance(); 
  }
}
```

Caveats
---------
Many argue that [parameters, local variables and instance variables should avoid using the `Optional` type](https://dzone.com/articles/java-8-optional-how-use-it). `Optional` is best used at the interface level between different methods or classes.

Using optionals, and especially throwing of exceptions, is not as performant as using null references. You should avoid premature micro-optimizations. But if you know that a certain piece of code is very latency sensitive, it is ok to use nulls for performance reasons.

----

*See other [recommended best practices here](https://github.com/RvPr/Awesome-Best-Practices/blob/master/README.md).*