Prefer Validating Assumptions
======================

Guideline
---------
If your method is assuming certain things about the inputs, or the state of the system, prefer validating those assumptions before proceeding.

Benefits
---------
User inputs can be unexpected and erraneous. Our code should check for and handle all possible errors in the user inputs. 

Even with assumptions that are based on purely internal invariants as opposed to user input, it is still preferable to validate them. Bugs occur because of false assumptions. Either because our assumptions were flawed to begin with. Or because of later changes made by other developers elsewhere in the system, that invalidated those assumptions. Bugs inevitably occur in all software systems, and we should write code accordingly to catch or mitigate them when they do occur.

By validating our assumptions, we will flag violations immediately, instead of proceeding with false assumptions. This makes debugging easier, as the error is thrown closer to the root-cause, and also because the error is more likely to be caught during development as opposed to prod. In addition, alerting the user of errors and/or using appropriate fallback logic, is preferable to silently returning the wrong result, or corrupting system data.

References
---------

1. [Official java guidelines on using asserts](https://docs.oracle.com/javase/7/docs/technotes/guides/language/assert.html)
1. [JavaPractices recommendation on validating class invariants](http://www.javapractices.com/topic/TopicAction.do?Id=6)
1. [StackExchange discussion around using invariants to guide software design](https://softwareengineering.stackexchange.com/questions/32727/what-are-invariants-how-can-they-be-used-and-have-you-ever-used-it-in-your-pro)

Examples
---------
Bad:
```java
double getFirstBalance() {
  // This method will never be called when the collection is empty
  return startSentinel.next().getValue();
}
```

Better:
```java
double getFirstBalance() {
  Preconditions.checkArgument(startSentinel.next() != endSentinel, 
    "This method should not be called when the collection is empty");
  return startSentinel.next().getValue();
}
```

Caveats
---------
If the code in question is very performance sensitive, you may want to skip the validation, in order to improve performance. Note however, the warning to [avoid premature optimizations](https://github.com/RvPr/Awesome-Best-Practices/blob/master/design/avoid-premature-optimizations.md).

If the assumption being made is "very obviously true", or if it is already being enforced by other code within the same class, and very close in the call-stack, it may be preferable to avoid extra checks, and prefer conciseness. However, if it takes you more than a few seconds to prove that something will always be true, it is likely not "obvious".

----

*See other [recommended best practices here](https://github.com/RvPr/Awesome-Best-Practices/blob/master/README.md).*