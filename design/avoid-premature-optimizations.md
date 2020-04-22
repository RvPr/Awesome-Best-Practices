Avoid Premature Optimizations
=============================

Guideline
---------
Avoid "optimizing" code until you're confident that it will be a performance bottleneck. "Optimizing" in this context refers to *"activities such as counting cycles and instructions in assembly language code."*

Benefits
---------
Optimizing code often takes up developer time. It also often comes at the cost of complexity and readability. 

The majority of the time, the resulting performance benefits are trivial and not noticeable.

References
---------
- ["Premature optimization is the root of all evil"](https://en.wikipedia.org/wiki/Program_optimization#When_to_optimize)
  
Examples
---------
Bad:
```java
// Using Arrays to minimize access time and heap-space 
static final String[] NAMES = new string[] {"Rajiv", "Roger", "Riu"}; 
```

Better:
```java
static final Collection<String> NAMES = ImmutableList.of("Rajiv", "Roger", "Riu");
```

----------

Bad:
```java 
boolean usernameIsInvalid(String username) {
  // First check the String's hashcode, since it is already cached, and can be compared in constant time
  return RESERVED_NAME.hashCode() == username.hashCode()
    && RESERVED_NAME.equals(username);
} 
```

Better:
```java 
boolean usernameIsInvalid(String username) {
  return RESERVED_NAME.equals(username);
} 
```

Caveats
---------
[This article](https://ubiquity.acm.org/article.cfm?id=1513451) covers the many caveats and misuses of this guideline. Notably:
- "Optimization" refers to micro-improvements such as shaving a few cycles or assembly instructions. If the optimized code confers major performance improvements (eg, bubble-sort vs quicksort), it should be strongly considered
- This guideline should be applied towards coding-implementation, not system design. Some upfront designing can confer significant performance benefits, and is much harder to retroactively implement in future 
- Even minor performance optimizations should be implemented sometimes, if the code in question is shown to be in the hotpath, and will produce noticeable performance improvements for the overall system.

----

*See other [recommended best practices here](https://github.com/RvPr/Awesome-Best-Practices/blob/master/README.md).*