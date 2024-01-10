# Lab Report 1 - Remote Access and FileSystem (Week 1)
_By: Sachin Ramanathan_

## The `cd` command

### _no_ arguments

I navigated to the `/home/lecture1/messages` directory and used the `cd` command with no arguments here.

```
[user@sahara ~/lecture1/messages]$ cd
[user@sahara ~]$ pwd
/home
```

Using the `cd` command with no arguments from the working directory `/home/lecture1/messages`, I observed that this changes the current working directory back to the `/home` directory.
I checked the directory it is in after the change using the `pwd` command which prints `/home`.
This happens no matter which directory we are currently in, so it will be an easy way to return to `/home`.
This is not an error but is an intended behaviour/use of the `cd` command to return to the `/home` directory. I anticipate that this will be of use to me.

### Path to a _directory_ as an argument

I ran the `cd` command from the `/home` directory with the argument `lecture1`, which is a subdirectory within the `/home` directory.

```
[user@sahara ~]$ cd lecture1
[user@sahara ~/lecture1]$ pwd
/home/lecture1
[user@sahara ~/lecture1]$
```

I observed that this navigates to the `lecture1` folder which was within the `/home` directory so the current working directory becomes `/home/lecture1`
I anticipate that this command will be useful to navigate between directories from any directory.
This output was not an error but the intended use of the `cd` command.

### Path to a _file_ as an argument

I ran the `cd` command from the `/home/lecture1` directory, which contains a folder `messages`, and the files `README`, `Hello.java` and `Hello.class`.

```
[user@sahara ~/lecture1]$ cd Hello.java 
bash: cd: Hello.java: Not a directory
```

By using `cd` to change directory into Hello.java, I receive an **error** stating that `Hello.java` is not a directory.
I understand that this throws an error because the `cd` commnand is used to change the current working directory into the specified directory, but `Hello.java` refers to a 
singular file, not a direcory and hence the `cd` command is not able to `cd` into the given path.

## The `ls` command

### _no_ arguments

I navigated to the `/home/lecture1/messages` directory and used the `ls` command with no arguments here.

```
[user@sahara ~/lecture1/messages]$ ls
en-us.txt  es-mx.txt  fr.txt  zh-cn.txt
```

Using the `ls` command with no arguments from the working directory `/home/lecture1/messages`, I observed that this lists the files in the `messages` directory, which was the current working directory when I used the command.
Since there are four files in the `messages` subdirectory, it lists the names of each of these files in the output.
This is not an error but is an intended behaviour/use of the `ls` command to list the names of all the files/folders in the current working directory directory.

### Path to a _directory_ as an argument

I ran the `ls` command from the `/home` directory with the argument `lecture1`, which is a subdirectory within the `/home` directory.

```
[user@sahara ~]$ ls lecture1
Hello.class  Hello.java  messages  README
```

I observed that this outputs the names of all the files and folders in the `lecture1` folder which was within the `/home` directory.
This means that I can use the `ls` command with the path to a directory as an argument to be able to list the files in any directory that I specify, instead of only the current working directory as it does without any arguments
I anticipate that this command will be useful to list files in any directory.
This output was not an error but the intended use of the `ls` command.

### Path to a _file_ as an argument

I ran the `ls` command from the `/home/lecture1` directory, which contains a folder `messages`, and the files `README`, `Hello.java` and `Hello.class`.

```
[user@sahara ~/lecture1]$ ls Hello.java
Hello.java
```

By using `ls` with the filename `Hello.java`, it outputs the name of the file located at the path I passed to the commmand, which was `Hello.java`.
This is not an error, rather it is the intended behaviour of the `ls` command when the path to a file is passed as an argument to it.

## The `cat` command

### _no_ arguments

I navigated to the `/home/lecture1/messages` directory and used the `cat` command with no arguments here.

```
[user@sahara ~/lecture1]$ cat 
This is a test
This is a test
```

Using the `cat` command with no arguments from the working directory `/home/lecture1/messages`, I observed that this does not do anything immediately, but if I type in something and hit Enter, it repeats what I typed back. Here, I typed `This is a test` into the console and it repeated the same line.
I think `cat` without no arguments does not serve any purpose other than to repeat the text entered, which is probably because instead of reading from a file whose path is passed to the argument, because no path is passed to it, it defaults to taking input from the console/user and outputting it.
From the documentation of the `cat` command, I see that it states "With no FILE, or when FILE is -, read standard input.", so I infer that this is the intended behaviour of the command when there is no file input and therefore it is not an error according to the developers.

### Path to a _directory_ as an argument

I ran the `cat` command from the `/home` directory with the argument `lecture1/`, which is a subdirectory within the `/home` directory.

```
[user@sahara ~]$ cat lecture1/
cat: lecture1/: Is a directory
```

By using `cat` with a path to a directory as an argument, I receive an **error** stating that `lecture1/` is a directory.
I understand that this throws an error because the `cat` command is used to output the contents of the specified file(s), but `lecture1/` refers to a directory, not a file and hence the `cat` command is not able to output its contents.

### Path to a _file_ as an argument

I ran the `cat` command from the `/home/lecture1` directory, which contains a folder `messages`, and the files `README`, `Hello.java` and `Hello.class`.

```
[user@sahara ~/lecture1]$ cat Hello.java 
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class Hello {
  public static void main(String[] args) throws IOException {
    String content = Files.readString(Path.of(args[0]), StandardCharsets.UTF_8);    
    System.out.println(content);
  }
}
```

By using `cat` with the filename `Hello.java` as an argument, it outputs the entire contents of the file located at the path I passed to the commmand, which was `Hello.java`.
This output is the Java code that was contained within the `Hello.java` file.
This is not an error, rather it is the intended behaviour of the `cat` command when the path to a file is passed as an argument to it.
