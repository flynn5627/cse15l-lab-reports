# Lab Report 3 - File Exploration and Text Analysis from the Command Line (Week 5)

## Part 1

The buggy program I chose is the `filter` method in the `ListExamples` class, in `ListExamples.java`.

Here is the existing, buggy implementation of the method:

```java
// Returns a new list that has all the elements of the input list for which
// the StringChecker returns true, and not the elements that return false, in
// the same order they appeared in the input list;
static List<String> filter(List<String> list, StringChecker sc) {
  List<String> result = new ArrayList<>();
  for(String s: list) {
    if(sc.checkString(s)) {
      result.add(0, s);
    }
  }
  return result;
}
```
The intended purpose of this method is to take in a List of Strings as an input, and a StringChecker object whose `checkString` method is
used to evaluate whether a String is valid to filter, and return a list of valid Strings in the same order.

### The failure-inducing input

```java
import static org.junit.Assert.*;
import org.junit.*;
import java.util.ArrayList;
import java.util.List;

class TestChecker implements StringChecker{
    public boolean checkString(String s){
        return (s.length() == 5);
    }
}

public class ListTests{
    @Test
    public void testFilter2(){
        StringChecker sc = new TestChecker();
        List<String> testList = new ArrayList<>();
        testList.add("1");
        testList.add("12345");
        testList.add("34");
        testList.add("56789");
        List<String> result = ListExamples.filter(testList, sc);
        assertEquals(2, result.size());
        assertEquals("12345", result.get(0));
        assertEquals("56789", result.get(1));
    }
}
```
To test the program, I will need a List to test it on and a StringChecker object to pass as the argument. For this, I created a class named `TestChecker` which implements the `StringChecker`
interface, with a `checkString` method which validates a string based on its length. If the String has a length of 5, it is valid and should be filtered. If it is not, it should not appear in the
filtered list. Here, I initialize a String ArrayList as a List and add four String elements to it: `1`, `12345`, `34`, `56789`.

The intended output of calling the `filter` method with this List and this StringChecker should be a List containing two elements, `12345` and `56789` in that exact order, since the 
order should be preserved by the method.

This test case fails, in the second assert statement, and the third one would fail too. The output list is of size 2 correctly, so the first assert statemnent passes, but the order of the elements is reversed, and therfefore the 
expected and actual outputs in the second and third assert statements are different.

### The passing input

```java
import static org.junit.Assert.*;
import org.junit.*;
import java.util.ArrayList;
import java.util.List;

class TestChecker implements StringChecker{
    public boolean checkString(String s){
        return (s.length() == 5);
    }
}

public class ListTests{
    @Test
    public void testFilter1(){
        StringChecker sc = new TestChecker();
        List<String> testList = new ArrayList<>();
        testList.add("1");
        testList.add("12345");
        testList.add("34");
        List<String> result = ListExamples.filter(testList, sc);
        assertEquals(1, result.size());
        assertEquals("12345", result.get(0));
    }
}
```
To test the program, I will need a List to test it on and a StringChecker object to pass as the argument. For this, I created a class named `TestChecker` which implements the `StringChecker`
interface, with a `checkString` method which validates a string based on its length. If the String has a length of 5, it is valid and should be filtered. If it is not, it should not appear in the
filtered list. Here, I initialize a String ArrayList as a List and add three String elements to it: `1`, `12345`, `34`.

The intended output of calling the `filter` method with this List and this StringChecker should be a List containing one element, `12345`.

This test case passes without failure.

### The Symptom

Running both the test cases with JUnit:

<img width="873" alt="Screenshot 2024-02-07 at 11 39 05 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/87c26af6-7207-48f1-ae9a-fb833feb5aca">

We see that the passing `testFilter1` test case passes without failure, but we see the symptom of the bug in our code in the `testFilter2` test case.

The symptom is visible in the following two assert statements:
```java
assertEquals("12345", result.get(0));
assertEquals("56789", result.get(1));
```
JUnit says: `expected:<[12345]> but was:<[56789]>`, which means while the string `12345` was expected to be the element at index 0, it was `56789` instead, and this is vice versa 
in the next line. We see that the symptom can be generalized to say that the output is actually in reverse order, while we want the order of elements to be preserved.

### The bug

We can infer that the bug is caused by the following line of code:
```java
result.add(0, s);
```

This code inserts the element at the index 0, at the beginning of the list, always, effectively always prepending elements. Rather, we want to append elements to the end of the list
so that the same order of elements is maintained. This can be fixed by removing the index from the call to the add method, so that it appends by default.

Fixed line:
```java
result.add(s);
```

Fixed code:
```java
// Returns a new list that has all the elements of the input list for which
// the StringChecker returns true, and not the elements that return false, in
// the same order they appeared in the input list;
static List<String> filter(List<String> list, StringChecker sc) {
  List<String> result = new ArrayList<>();
  for(String s: list) {
    if(sc.checkString(s)) {
      result.add(s);
    }
  }
  return result;
}
```

### Why this helps

This fixes it because the issue with the code prepending new elements is resolved, since now it is made to append the new element in all cases, effectively solving the issue we 
recognised from seeing the symptom, which was that the elements were reversed in order in the filtered list.


## Part 2
