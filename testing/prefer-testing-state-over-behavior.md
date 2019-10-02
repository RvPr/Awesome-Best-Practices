Entry Name
============

Guideline
---------
Instead of trying to test implementation details, it is better to test the final result and return values.

This implies avoiding mocks, and preferring Fakes.

Benefits
---------
Tests are easier to understand, and less brittle, when they test for the end-results that we care about, as opposed to the implementation details.

References
---------
- [Google testing guidelines](https://testing.googleblog.com/2013/03/testing-on-toilet-testing-state-vs.html)
- [SendGrid blog](https://sendgrid.com/blog/when-writing-unit-tests-dont-use-mocks/)

Examples
---------
Bad:
```java
@Test
public void nameLookup_shouldFindNameFromUsername() {
  Datastore mockDatastore = Mockito.mock(Datastore.class);
  when(mockDatastore.getName(anyString())).thenReturn("Rajiv Prabhakar");
  NameClient client = new NameClient(mockDatastore);
  
  String firstName = client.getFirstName("prarajiv");

  verify(mockDatastore, times(1)).getName("prarajiv");
  assertThat(firstName).isEqualTo("Rajiv");
}
```

Better:
```java
@Test
public void nameLookup_shouldFindNameFromUsername() {
  Datastore fakeDatastore = new fakeDatastore().withEntry("prarajiv", "Rajiv Prabhakar");
  NameClient client = new NameClient(fakeDatastore);
  
  String firstName = client.getFirstName("prarajiv");

  assertThat(firstName).isEqualTo("Rajiv");
}
```


Caveats
---------
Sometimes, the expected behavior also contains some "interactions" that are important, but do not produce a state change.

For example, consider a meta-sort method that is supposed to use BubbleSort for sizes < 10, and MergeSort for larger sizes. The final state/return is the same regardless of whether BubbleSort or MergeSort is used. Using a timed-benchmark would be ideal, but it is also reasonable to use Mocks to verify that BubbleSort isn't invoked for larger sizes.

----

*See other [recommended best practices here](TODO).*