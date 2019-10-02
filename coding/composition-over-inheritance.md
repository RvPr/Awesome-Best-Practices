Prefer Composition over Inheritance
================

Guideline
---------
Instead of extending another class, inject that class into your constructor instead. 
Ie, prefer composition over inheritance where possible.

Benefits
---------
Composition avoids the drawbacks associated with inheritance. 
Namely, inheritance makes the code more confusing and harder to understand. 
Inheritance also causes tight coupling between your subclass and its parent classes, including its internals.

Inheritance is often used to avoid code-duplication. But this can be equally well accomplished using composition.

References
---------

1. [Effective Java](https://www.amazon.com/Effective-Java-Joshua-Bloch-ebook/dp/B078H61SCH) - Item 18
1. [StackExchange discussion](https://softwareengineering.stackexchange.com/questions/134097/why-should-i-prefer-composition-over-inheritance)

Examples
---------

Bad:
```java
public class SimpleStack<T> {
    private final LinkedList<T> list = new LinkedList<>();

    public void push(T val) {
        list.addLast(val);
    }

    // Throws NoSuchElementException if stack is empty
    public T pop() {
        return list.removeLast();
    }
}

class SafeStack<T> extends SimpleStack<T> {
    // Returns null instead of throwing exception
    @Override
    public T pop() {
        try {
            return super.pop();
        } catch (NoSuchElementException e) {
            return null;
        }
    }
}

class LoggingSafeStack<T> extends SafeStack<T> {
    private static final Logger log = LogManager.getLogger(LoggingSafeStack.class);

    @Override
    public void push(T val) {
        super.push(val);
        log.info("Pushed " + val + " into stack");
    }

    @Override
    public T pop() {
        T value = super.pop();
        log.info("Popped value: " + value);
        return value;
    }
}

public static void main(String... args) {
    SimpleStack<Integer> exceptionThrowingStack = new SimpleStack<>();
    SafeStack<Integer> safeStack = new SafeStack<>();
    LoggingSafeStack<Integer> safeStackWithLogging = new LoggingSafeStack<>();
}
```

Better:
```java
interface Stack<T> {
    void push(T val);
    T pop();
}

class SimpleStack<T> implements Stack<T> {
    private final LinkedList<T> list = new LinkedList<>();

    @Override
    public void push(T val) {
        list.addLast(val);
    }

    @Override
    public T pop() {
        return list.removeLast();
    }
}

class SafeStack<T> implements Stack<T> {
    private final Stack<T> underlying;

    SafeStack(Stack<T> underlying) {
        this.underlying = underlying;
    }

    @Override
    public void push(T val) {
        underlying.push(val);
    }

    // Returns null instead of throwing exception
    @Override
    public T pop() {
        try {
            return underlying.pop();
        } catch (NoSuchElementException e) {
            return null;
        }
    }
}

class LoggingStack<T> implements Stack<T> {
    private static final Logger log = LogManager.getLogger(LoggingStack.class);

    private final Stack<T> underlying;

    LoggingStack(Stack<T> underlying) {
        this.underlying = underlying;
    }

    @Override
    public void push(T val) {
        underlying.push(val);
        log.info("Pushed " + val + " into stack");
    }

    @Override
    public T pop() {
        T value = underlying.pop();
        log.info("Popped value: " + value);
        return value;
    }
}

public static void main(String... args) {
    Stack<Integer> exceptionThrowingStack = new SimpleStack<>();
    Stack<Integer> safeStack = new SafeStack<>(exceptionThrowingStack);
    Stack<Integer> safeStackWithLogging = new LoggingStack<>(safeStack);

    // Not possible in the inheritance example
    Stack<Integer> exceptionThrowingStackWithLogging = new LoggingStack<>(exceptionThrowingStack);
}
```

Caveats
---------
If the class/interface in question has an extremely long list of methods, and the behavior you want to change only impacts one of those methods, it would be very verbose to use composition instead of inheritance.
In such cases, if the scope of your new class is very limited, you may want to use inheritance instead, in the interests of brevity.

----

*See other [recommended best practices here](TODO).*