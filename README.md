# Scripting in Bash

## Introduction to scripts

- What is a script?
  - Fundamentally, a bash script is just a file containing a series of bash commands.
  - Scripts are formatted as text files. But the things in this file are special.
  - The first line of a script file tells the computer in which language (i.e., shell) we're writing our script. This line starts with `#!` - also known as a shebang. The shebang tells Terminal that we're about to indicate which language we're going to use.
  - Follow the shebang with the path to the shell that you'd like to use. Yes, the shell itself is a program!
    - `#! /bin/bash`
  - Let's start by creating your first script - `myScript.sh`
    - `nano myScript.sh`
    - Add the shebang line
    - Add two commands in the body of the file
      - `echo "Hello, "$USER"!"`
      - `echo "I'M A SCRIPT AND I WORK!"`

- Command-line Arguments
    - To access the argument from inside the script, bash reserves the special variables `$1`, `$2`, `$3`, ...
    - For practice, go back to `myScript.sh` that we created earlier and change `$USER` to `$1`. Now, run it by typing `myScript.sh <YOUR_NAME>`

## Downloading a file from the command line

The data file to be used in your assignment is available here: [chiari.summary_statistics.csv](https://raw.githubusercontent.com/IntroToCompBioLSU-Spr20/Scripts2_Week4/master/chiari.summary_statistics.csv)

```
Assignment 3



Will be due Friday, Feb. 7th at noon.
```
