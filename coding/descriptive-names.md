Always Use Descriptive Names
==================

Guideline
---------
1. *"The name of a variable/function/class should tell you why it exists, what it does, and how it is used"*
1. Avoid names that are misleading or false.
1. Avoid abbreviations/acronyms that aren't universally known.
1. Avoid giving 2 different variables/functions/classes names that sound similar.
1. Avoid prefixes/suffixes that are redundant given the context.
1. Avoid [Hungarian Notation](https://en.wikipedia.org/wiki/Hungarian_notation#Examples) in typed languages like Java.
1. Avoid member prefixes like _ or m (in Java at least)
1. Be consistent. Pick a name for a specific concept, and then stick to it. Conversely, avoid using the same name for distinct concepts.

Benefits
---------
Code is much easier/faster to read and understand, especially for new team members, when above guidelines are used.

Any potential bugs are also easier to spot as a result.

References
---------
[Clean Code](https://www.goodreads.com/book/show/3735293-clean-code) - chapter 2

Examples
---------

Bad: `int d; // elapsed Time in Days`

Better: `int elapsedTimeInDays;`

---

Bad: `void copyChars(char[] c1, char[] c2);`

Better: `void copyChars(char[] source, char[] dest);`

---

Bad: `String getDisplay();  // Also changes state`

Better: `String updateDisplay();`

---

Bad: `dpUrl;`

Better: `detailPageUrl;`

---

Bad:
```java
String name;
String theName;
```

Better:
```java
String nickname;
String legalName;
```

---

Bad:
```java
class Product {
  String _productNameString;
  int _productCostInteger;
}
```

Better:
```java
class Product {
  String name;
  int cost;
}
```

---

Bad:
```java
String getName();
int fetchCost();
LocalDate retrieveDate();
```

Better:
```java
String getName();
int getCost();
LocalDate getDate();
```

---

Bad:
```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<>();
  for (int[] x : theList)
    if (x[0] == 4)
      list1.add(x);
  return list1;
}
```

Better:
```java
public List<Cell> getFlaggedCells() {
  List<Cell> flaggedCells = new ArrayList<>();
  for (Cell cell : gameBoard)
    if (cell.isFlagged())
      flaggedCells.add(cell);
  return flaggedCells;
}
```

Caveats
---------
Too much verbosity hurts readability as well. Strike a balance between ambiguity and verbosity. 
The smaller the scope of a variable, the more concise its name can be.

----

*See other [recommended best practices here](https://github.com/RvPr/Awesome-Best-Practices/blob/master/README.md).*