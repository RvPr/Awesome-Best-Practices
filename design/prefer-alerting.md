Prefer Failing/Alerting over Silently Hiding Potential Bugs
======================

Guideline
---------
Don't use fallbacks in such a way that potential bugs are hidden.
Ideally, log something that can be monitored and alarmed on. 
Or if you're sufficiently confident that something will never happen, fail fast instead.

Benefits
---------
Fallbacks are bad for 2 reasons. 
First, the UX they provide is worse than the "main" UX. 
Second, they also add more complexity to the code.

For these reasons, it's best to avoid unnecessary fallbacks where possible. 
And where fallbacks are indeed needed, ensure that alarms get triggered whenever the fallback is used - otherwise, the bug will never be surfaced and fixed.

References
---------
1. [The original "Fail fast" essay by Jim Shore](https://www.martinfowler.com/ieeeSoftware/failFast.pdf)
1. [DZone summary](https://dzone.com/articles/fail-fast-principle-in-software-development)

Examples
---------
Bad:
```java
String getGreeting() {
  try {
    return "Hello " + getName();
  } catch (Exception e) {
    return "Hello";
  }
}
```

Better (with alerting):
```java
String getGreeting() {
  try {
    return "Hello " + getName();
  } catch (Exception e) {
    pageOps(e);
    return "Hello";
  }
}
```

Better (with fail-fast):
```java
String getGreeting() {
  return "Hello " + getName();
}
```

Caveats
---------
There are a couple factors to consider when failing-fast:
1. How confident are you that the error will not happen?
1. If it does happen and you've thrown a failure, how significant will the impact be?
1. Is there a good fallback-logic you can use, when encountering a failure?

If you're very confident, and/or the potential impact is limited, and/or there is no good fallback available => failing fast is a good way to keep your code clean, and prevent unintended side-effects.

Otherwise, using a fallback with an alerting mechanism, can prevent disastrous outages in the short-term. 
You can also use this as an intermediate step, and fail-fast at a later date once you have data showing that the error-case has never happened.

----

*See other [recommended best practices here](https://github.com/RvPr/Awesome-Best-Practices/blob/master/README.md).*