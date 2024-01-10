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
[user@sahara ~]$ cd lecture1/
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
