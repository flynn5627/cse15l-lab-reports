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

The command I chose is the **grep** command. This command searches the given input files, and outputs the lines from the input files that match the given pattern.

## grep: the `-i` option

By using `man grep`, I learnt that there exists the option `-i` which means "ignore case". This allows you to ignore differences in case (upper and lower case) when finding matches
for a particular text search.

```bash
bash-3.2$ grep "the" technical/911report/chapter-1.txt | wc -l
     313
bash-3.2$ grep "the" technical/911report/chapter-1.txt -i | wc -l
     323
```

In this case, I was in the docsearch directory and was trying to search for lines containing the word "the" in the file `technical/911report/chapter-1.txt` using the grep command.
I piped this output to `wc -l` which outputs the number of lines in the output of grep, which corresponds to the number of matching lines.

When I used this command first, without the `-i` flag, it only matched 313 lines. However, when I added the `-i` flag, this increased to 323. This is because without the ignore case flag, it does not match instances where the word is used in mixed case or upper case, such as the word "The", with the first letter capitalised when it is at the beginning of the sentence. Ignore case (`-i`) is very useful for this because it allows me to ignore the case when matching in cases where it's irrelevant to me like this.

Here is a preview of the output of `grep -i` without piping it to `wc`:
```bash
bash-3.2$ grep "the" technical/911report/chapter-1.txt -i
    Tuesday, September 11, 2001, dawned temperate and nearly cloudless in the eastern United States. Millions of men and women readied themselves for work. Some made their way to the Twin Towers, the signature structures of the World Trade Center complex in New York City. Others went to Arlington, Virginia, to the Pentagon. Across the Potomac River, the United States Congress was back in session. At the other end of Pennsylvania Avenue, people began to line up for a White House tour. In Sarasota, Florida, President George W. Bush went for an early morning run.
...
```

As another example, similarly, if I wanted to see the results for "us" regardless of the case, I could use the following command:
```bash
bash-3.2$ grep "us" technical/911report/chapter-1.txt -i
    Tuesday, September 11, 2001, dawned temperate and nearly cloudless in the eastern United States. Millions of men and women readied themselves for work. Some made their way to the Twin Towers, the signature structures of the World Trade Center complex in New York City. Others went to Arlington, Virginia, to the Pentagon. Across the Potomac River, the United States Congress was back in session. At the other end of Pennsylvania Avenue, people began to line up for a White House tour. In Sarasota, Florida, President George W. Bush went for an early morning run.
    In another Logan terminal, Shehhi, joined by Fayez Banihammad, Mohand al Shehri, Ahmed al Ghamdi, and Hamza al Ghamdi, checked in for United Airlines Flight 175, also bound for Los Angeles. A couple of Shehhi's colleagues were obviously unused to travel; according to the United ticket agent, they had trouble understanding the standard security questions, and she had to go over them slowly until they gave the routine, reassuring answers.
...
```

## grep: the `-c` option

By using `man grep`, I learnt that there exists the option `-c` which means "count". This allows you to only get the number of matched lines as the output as opposed to a list of every line, which may be quite long and irrelevant output in some cases.

```bash
bash-3.2$ grep "the" technical/911report/chapter-1.txt -c
313
```

Taking the same example of counting the number of instances of the word "the" in the file located at `./technical/911report/chapter-1.txt`, I see that the `-c` flag MASSIVELY makes my previous example easier. As opposed to piping the output to `wc -l`, the `-c` flag enables `grep` to do this task by itself, making it very useful when you want to see the number of  results for a search instead of a list of all the matching results.

As another example, similarly, I can use the same option to count the number of occurences of the word "us" in the same file:

```bash
bash-3.2$ grep "us" technical/911report/chapter-1.txt -c
139
```

## grep: the `-n` option

By using `man grep`, I learnt that there exists the option `-n` which means "line number". This makes it such that `grep` now displays the line number preceeding each matched line in the output.

```bash
bash-3.2$ grep "the" technical/911report/chapter-1.txt -n
6:    Tuesday, September 11, 2001, dawned temperate and nearly cloudless in the eastern United States. Millions of men and women readied themselves for work. Some made their way to the Twin Towers, the signature structures of the World Trade Center complex in New York City. Others went to Arlington, Virginia, to the Pentagon. Across the Potomac River, the United States Congress was back in session. At the other end of Pennsylvania Avenue, people began to line up for a White House tour. In Sarasota, Florida, President George W. Bush went for an early morning run.
8:    For those heading to an airport, weather conditions could not have been better for a safe and pleasant journey. Among the travelers were Mohamed Atta and Abdul Aziz al Omari, who arrived at the airport in Portland, Maine.
12:Boarding the Flights
...
```

Taking the same example of finding the lines containing the word "the" in the file located at `./technical/911report/chapter-1.txt`, I see that the `-n` optionmakes it very easy to locate matching lines. This enables us to easily locate these lines in their source file by the line number rather than visually look for the line by its contents when the line numbers were not provided, which may be difficult sometimes. For example, I can see here that one of the lines that contains "the" and is a match, "Boarding the Flights" can be found at line number 12 from the output of grep.

As another example, similarly, I can use the same option to find the number of occurences of the word "us" along with their line numbers in the same file. We see that below, it's found on lines 6, 24, and more.

```bash
bash-3.2$ grep "us" technical/911report/chapter-1.txt -n
6:    Tuesday, September 11, 2001, dawned temperate and nearly cloudless in the eastern United States. Millions of men and women readied themselves for work. Some made their way to the Twin Towers, the signature structures of the World Trade Center complex in New York City. Others went to Arlington, Virginia, to the Pentagon. Across the Potomac River, the United States Congress was back in session. At the other end of Pennsylvania Avenue, people began to line up for a White House tour. In Sarasota, Florida, President George W. Bush went for an early morning run.
24:    In another Logan terminal, Shehhi, joined by Fayez Banihammad, Mohand al Shehri, Ahmed al Ghamdi, and Hamza al Ghamdi, checked in for United Airlines Flight 175, also bound for Los Angeles. A couple of Shehhi's colleagues were obviously unused to travel; according to the United ticket agent, they had trouble understanding the standard security questions, and she had to go over them slowly until they gave the routine, reassuring answers.
...
```

## grep: `-m num` option

By using `man grep`, I learnt that there exists the option `-m num` which means "maximum", used with an integer argument. This makes it such that `grep` now displays only `num` number of matches, and terminates execution after that.
```bash
bash-3.2$ grep "the" technical/911report/chapter-1.txt -m 2
    Tuesday, September 11, 2001, dawned temperate and nearly cloudless in the eastern United States. Millions of men and women readied themselves for work. Some made their way to the Twin Towers, the signature structures of the World Trade Center complex in New York City. Others went to Arlington, Virginia, to the Pentagon. Across the Potomac River, the United States Congress was back in session. At the other end of Pennsylvania Avenue, people began to line up for a White House tour. In Sarasota, Florida, President George W. Bush went for an early morning run.
    For those heading to an airport, weather conditions could not have been better for a safe and pleasant journey. Among the travelers were Mohamed Atta and Abdul Aziz al Omari, who arrived at the airport in Portland, Maine.
```

This is very useful in many cases because even if there might be thousands of matches for the search query, you might only want/need some number of them. In my example of finding the lines containing the word "the" in the file located at `./technical/911report/chapter-1.txt`, if I only wanted to see two matches, I could use the option `-m 2` for grep. We can see in the output that only two lines have been printed due to this reason, although there are many more matched lines as we know.

As another example, if we wanted only one match, for the word "us", we can use the below command. It prints the first found match for the word.
```bash
bash-3.2$ grep "us" technical/911report/chapter-1.txt -m 1
    Tuesday, September 11, 2001, dawned temperate and nearly cloudless in the eastern United States. Millions of men and women readied themselves for work. Some made their way to the Twin Towers, the signature structures of the World Trade Center complex in New York City. Others went to Arlington, Virginia, to the Pentagon. Across the Potomac River, the United States Congress was back in session. At the other end of Pennsylvania Avenue, people began to line up for a White House tour. In Sarasota, Florida, President George W. Bush went for an early morning run.
```
