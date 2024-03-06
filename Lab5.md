# Lab Report 5

## Part 1 – Debugging Scenario

### The Original Post

Title: **Output redirection not working!**

Username: `somerandomguy123`

Description:

I have a simple bash script that runs a Java file and redirects its output to a file named `test.txt`. But when I try to run this command, it just says the command is not found.

Is there a problem with ieng6 where output redirection doesn't work? I know that the java commands sometimes don't work on ieng6, is this connected to that?

![image](https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/79dedc7b-a176-4971-8c4a-7aac345e1731)

### Response from the TA

Hi `somerandomguy123`. To understand the issue, we'll need some additional context about the contents of the files involved and the commands you were trying to run. 

We cannot infer the underlying bug that is causing this error message to be displayed without knowing the commands that were run, which are contained in the `test.sh` file.

Can you provide a screenshot of the contents of this file by running `cat test.sh`?


### Response from Student

Here's the output of the command. 

<img width="353" alt="Screenshot 2024-03-06 at 11 38 43 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/49c8dc9c-4ca6-41dc-8534-c767cb4d13a8">

### The Bug & Response from the TA

Hi `somerandomguy123`, from this information I can see that the issue is how you're attempting to redirect the output to the file. For this, we use the output redirecting operator, `>`.

Since you attempted to do this using the pipe operator, `|`, which pipes the output of one *command* to another *command*, it assumes the name of the file you provided is a *command* rather than the file to redirect the output to. This is why you received the "command not found" error message.

To resolve this, you can replace the `|` operator with `>` and the bash script should work as intended.

### The Setup

#### File Structure

To setup this case, I needed to create two files in the current working directory-`Test.java` and `test.sh`:

<img width="278" alt="Screenshot 2024-03-06 at 11 46 26 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/1945ff24-d1bd-434f-b7aa-c4dc793f6db1">

#### `Test.java`

Test.java is a regular "Hello World" program written in Java. Its contents are provided before

```java
class Test{
        public static void main(String[] args){
                System.out.println("Hello world");
        }
}
```

#### `test.sh`

test.sh is a bash script that compiles all the Java files in the current working directory and *attempts* to run the Test class and store its output in the file `test.txt`.

```bash
javac *.java
java Test | test.txt
```

#### Triggering the bug

To trigger the bug, you simply need to run the bash script `test.sh` using the following command:

```bash
bash test.sh
```

#### Fixing the bug

To resolve this bug, you need to replace the `|` operator in the second line of `test.sh` with `>`. This can be done by using `nano`, `vim`, an IDE, or any text editor to edit the file.

## Part 2 - Reflection

I learnt a lot from lab these past few weeks. Learning how to make our own autograder was incredibly fun, and it's very cool to understand how the autograder that I have grown very frustrated with
in my CSE 12 class works internally. Knowing the internal implementation using bash and java allows me to better understand why doing certain things in my code causes the autograder to fail.

Moreover, I learnt a lot about bash and now feel confident in combining a variety of different commands in the same script to achieve what I want to do. 

Learning `vim` was fun since I've seen the memes about how you can't exit vim and the best way to exit vim is to just buy a new computer, so it was cool to actually learn how to use vim
and its different modes, and now I can finally understand the frustration behind the meme.

I'm sure that this class will be one of the most easy but useful classes for the rest of my computer science career.
