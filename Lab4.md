# Lab Report 4 - Vim (Week 7)

My personal record for this series of steps is **1:45**.

## Step 4 - SSH-ing into `ieng6`
My pre-existing SSH Config File:

<img width="249" alt="Screenshot 2024-02-21 at 10 58 37 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/e7514a74-00ca-4dc6-88c8-97ef81b1226e">

Since I had previously added the instructions for logging into the ieng6 server through SSH into the SSH config file, to login to SSH, I simply needed to type 
`ssh`, `<Space>`, and `ieng6server` because ieng6server is what I previously named the configuration as.

This command therefore successfully connected to my ieng6 instance through SSH.
<img width="926" alt="Screenshot 2024-02-21 at 11 00 31 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/261e4d1b-3bfc-4727-b5d2-ff35cea50de9">


## Step 5 - Cloning the repository

To clone the repository from GitHub onto my ieng6 server instance, I used the `git clone` command, and used the SSH link which I had previously copied from GitHub.
The keystrokes I used were typing `git clone`, `<Space>` and then `<Command>`+`<V>` to paste the link. I then pressed `<Enter>` to run the command, which cloned the repository into a 
new directory named `lab7` in my current working directory.

<img width="553" alt="Screenshot 2024-02-21 at 10 56 58 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/24151930-6d22-46d0-941b-754994f0c85d">

I then type `cd lab7` to change the working directory into lab7 to make my future commands easier to write.

<img width="286" alt="Screenshot 2024-02-21 at 11 03 30 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/7cb92395-9776-47d7-9767-540c101816f6">

## Step 6 - Run the tests, demonstrating that they fail

To run the tests, I simply use `bash` to run the `test.sh` shell script. To do this, I type `bash` and then `<Space>`. To type in the name of the file, I use a bash shortcut
by simply typing the letter `t`, and then pressing `<Tab>` to autocomplete the whole file name, which is `test.sh`. Running this command, the bash script which executes, in turn
compiles the Java files in the current directory and runs the JUnit tests in the `ListExamplesTests` class.

We therefore see that two tests ran and one of them has failed.
<img width="620" alt="Screenshot 2024-02-21 at 11 07 06 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/1a514c27-e836-45c2-9a97-e7ec9dd2748a">

## Step 7 - Edit the code file to fix the failing test

### Step 7.1 - Entering `vim`
To resolve the issues, we need to edit `ListExamples.java` to correct the buggy implementation. In order to do this, we can use `vim` as a text editor. I typed `vim` and `<Space>`,
and then to type in the file name, I simply typed `L` (capital L by typing `<Shift>`+`L`) and then pressed `<Tab>`, which autocompleted the filename until `ListExamples`.

<img width="371" alt="Screenshot 2024-02-21 at 11 11 55 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/4efba432-ac3e-44ea-8f04-bad8d026a77a">

It does not autocomplete the filename completely as there exists both `ListExamples.java` and `ListExamplesTests.java` which match these starting characters.
To continue, I type `.`, and upon pressing `<Tab>` again, it autocompletes the entire file name (`ListExamples.java`) as this resolves the ambiguity between the two matching files.
Now that the whole command is typed, I can run it to enter `vim`

<img width="409" alt="Screenshot 2024-02-21 at 11 13 55 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/cbb331bc-54f7-4744-bbb6-497b4473e735">

<img width="952" alt="Screenshot 2024-02-21 at 11 14 35 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/33d42624-34a9-42f5-972c-049a7ce76d71">

### Step 7.2 - Getting to the correct line
To get to the bottom, which is closer to the line with the bug, I type `:` (which itself is a combination of `<Shift>`+`;`) and then `$` (which is a combination of `<Shift>`+`4`). 
This `:$` command gets me to the end of the file.

<img width="954" alt="Screenshot 2024-02-21 at 11 16 57 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/a1f30b60-2e2d-4358-9cae-8705d6bd6ad2">

Then, to move up to the correct line, I type `6k`. The number `6` before the command indicates that it should be repeated 6 times, and the command `k` moves up a line. Therefore, 
this combination of the characters results in moving up 6 lines, which takes us directly to the line with the bug.

<img width="959" alt="Screenshot 2024-02-21 at 11 19 11 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/02ef0723-7875-49bf-9108-f4f98085d1e8">

### Step 7.3 - Getting to the correct position to edit

First, I type `w`, which brings me to the beginning of the next word, which is `index1`, which is the word we want to edit. The cursor is now at `i`.

<img width="963" alt="Screenshot 2024-02-21 at 11 20 25 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/f9c22440-614c-4076-b2f9-f751ccb63809">

Then, I type `e`, which brings me to the end of the current word, which brings the cursor to the number `1`.

<img width="941" alt="Screenshot 2024-02-21 at 11 21 10 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/911a93bb-26d1-48a2-b098-8e65e8df5e51">

### Step 7.4 - Making the edits

To delete the 1, I type `x` since we are in normal mode. This results in only having `index`, with our cursor after the letter x.

<img width="952" alt="Screenshot 2024-02-21 at 11 22 46 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/5d380ca4-4134-4003-9b88-8e80a0196343">

Then, to insert the 2 in the same position, I enter into insert mode by typing `i`, and then I type the `2` to be entered.

<img width="953" alt="Screenshot 2024-02-21 at 11 23 47 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/08b7f135-9eb4-4e29-a6aa-1b0271c31b6c">

### Step 7.5 - Save and Quit

Finally, to exit insert mode, I press `<Escape>` which brings me back to normal mode. To exit, I type the sequence of characters `:wq`, and then press `<Enter>` to execute the commands.
The `w` saves the file and the `q` quits the vim editor. The interface returns back to the normal terminal view after this.

<img width="948" alt="Screenshot 2024-02-21 at 11 25 25 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/bfccf620-9f33-4443-9c2a-abaabc56eb45">

## Step 8 - Run the tests, demonstrating that they now succeed

To run the tests, I simply use `bash` to run the `test.sh` shell script. To do this, I type `bash` and then `<Space>`. To type in the name of the file, I use a bash shortcut
by simply typing the letter `t`, and then pressing `<Tab>` to autocomplete the whole file name, which is `test.sh`. Running this command, the bash script which executes, in turn
compiles the Java files in the current directory and runs the JUnit tests in the `ListExamplesTests` class.

We therefore see that two tests ran and both of them have now passed, since we have fixed the bug.

<img width="372" alt="Screenshot 2024-02-21 at 11 26 35 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/e9e74941-9ea2-41ad-91c8-eb2ae1750c2c">

## Step 9 - Commit and push to GitHub

### Step 9.1 - Stage the changes
To stage our changes for committing, I type the command `git add .` which stages all the changes in the current directory for committing.

<img width="329" alt="Screenshot 2024-02-21 at 11 28 43 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/6c5f7c15-ff31-4c85-906b-cdea20c1da2b">

### Step 9.2 - Commit the changes
To commit my changes, I type `git commit -m "Fixed bugs"`, which commits my staged changes with the commit message "Fixed bugs".

<img width="497" alt="Screenshot 2024-02-21 at 11 29 00 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/4d44d172-bb41-49e9-8631-996d8e61d484">

### Step 9.3 - Push the changes
Finally, to push my changes to GitHub, I type `git push origin main`, which pushes all the changes to the main branch of the source repository on GitHub, which executes successfully.

<img width="722" alt="Screenshot 2024-02-21 at 11 30 18 AM" src="https://github.com/flynn5627/cse15l-lab-reports/assets/156235257/ded18527-6077-4064-b5bf-746a8d6b7583">
