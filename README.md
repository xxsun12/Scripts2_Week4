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

While we could copy and paste the contents of this file into a new file using the graphical interface, this approach becomes  tedious and difficult with lots of files or large files. Instead, we'd like to be able to download the file contents directly from the command line. To do this, we can use the `curl` command. To start, try executing this command

`curl https://raw.githubusercontent.com/IntroToCompBioLSU-Spr20/Scripts2_Week4/master/chiari.summary_statistics.csv`

What happens? How could we save the contents that we're downloading to a file directly?

## Command line find-and-replace

You'll often need to clean up the contents of data files before doing additional analyses. In this case, we'd like to replace commas with spaces. To do this from the command line, we can use a powerful tool called `sed`. `sed`, which is short for "stream editor", can do many different things, but we'll just use it for simple find-and-replace for now. Try executing this

`sed 's/,/ /g' chiari.summary_statistics.csv`

What do you see? What's different compared to the original contents of the file?

What about when you run this?

`sed -i "_backup" 's/,/ /g' chiari.summary_statistics.csv`

Note that this syntax will stay the same for any find and replace operation that we want to do. The only thing that will change is the text to find and replace (between the slashes).

```
Assignment 3

(1) Download and save the file above (`chiari.summary_statistics.csv`)
(2) Count the lines in the file and store this value in a variable
(3) Using this value and tail, extract all the lines in the file except the header (top) line, then pipe to -->
(4) grep to extract lines ending with an HoT alignment score of 1, then pipe to -->
(5) grep to extract lines with 14 taxa (think about what pattern would allow this) -->
(6) then extract and save only the gene names from these lines

After you've worked out the commands needed to do this, put them in a script and make sure you can execute it.

Now, generalize your script to accept any HoT score and number of taxa as command-line arguments.

Turn in this generalized script in the homework folder and submit via a pull request.

Will be due Friday, Feb. 7th at noon.
```

## Math in Bash

In general, bash isn't very good for mathematical operations, but it can be done in several ways. Probably the easiest is to wrap a mathematical expression in _double_ parentheses and precede it with a `$`, since you pretty much always want the _value_ of the mathematical result.

- `echo $(( 2 + 2 ))`
  - `myNum = $(( 2 + 2 ))`
- `echo $(( 3 * 6 ))`
- `echo $(( 20 / 3 ))`
- `echo $(( 20 % 3 ))`

Compare the output of these last two lines? What's going on?

NOTE: bash can only handle _integers_ and not floating-point numbers (i.e., decimals)

Try this. What happens?

myVar=3
echo $myVar
((myVar++))
echo $myVar
((myVar++))
echo $myVar

What does the `++` operator do?

```
Practice Exercise - Math and Scripting

(1) Create a new script file. Be sure to add the shebang line and set permissions properly to execute.
(2) Allow your script to accept two command-line arguments that are numbers.
(3) Inside the script, multiply the two numbers and then subtract the second number from the product
(4) Print the result to the screen.

```

## If...Else

One of the other very important programming concepts is known as "flow control". Basically, this just means doing different things depending on current conditions. There are several ways to accomplish flow control, but probably the most common is the `if...else` statement, which looks like this:

```
if <TEST_SOMETHING>
then
  <DO_THING_1>
else
  <DO_THING_2>
fi
```

There are lots of different ways to compare values, depending on what type of values you're working with. Here is a page that lists several options: [Bash comparison operators](http://tldp.org/LDP/abs/html/comparison-ops.html). Here's one example:

```
# Defining value of numeric variable
a=2
        
# if...else to see if value of number is at least 3
if [ $a -lt 3 ]  # Note: this could also be ((a < 3))
then
  echo "$a is less than 3."
else
  echo "$a is NOT less than 3."
fi
```

Here's an example of combining backticks, command-line arguments, and an if...else statement to write a script that tests if a command-line argument has a certain number of characters.

```
# Recording length of word provided on command line
# What's going on with the backticks (``) here?
myWordLength=`echo -n $1 | wc -m`

# test if word is at least 5 characters
if [ $myWordLength -lt 5 ]
then
  echo "$1 is shorter than 5 characters."
else
  echo "$1 is at least 5 characters in length."
fi
```

```
Practice Exercise - If...Else

(1) Start with the script you wrote above to practice math
(2) Now add two if...else statements to check that:
  - The first number is between 3 and 7
  - The second number is between 10 and 14
(3) If either of these conditions are not met, have your script print an error message to the screen.
```

## For Loops

One of the most common reasons to write a script is to automate something that is, at a minimum, very tedious to do manually and, at worst, completely impossible otherwise. A versatile way to incorporate repitition into a script is to use a `for` loop. `for` loops in bash have the following structure:

```
for num in 1 two 3 FOUR
do
  echo $num
done
```

Let's break this down. First, we've defined a new variabled named `num`. This variable can be named anything you want. In this case, `num` will iteratively take the value of anything included in the list that follows `in`. During each iteration, the code in between `do` and `done` will be executed. In this case, we will simply print out each of the values our variable takes, one after the other. Later, we will use `for` loops that have a whole series of commands inside the loop.

Double parentheses notation can also be used to write a `for` loop in a way that doesn't require you to write out every unique element in the list:

```
for ((num=1;num<=10;num++))
do
  echo $num
done
```

When written this way, the `for` loop statement has a structure like this

```
for (( <START_VALUE> ; <STOP_CONDITION> ; <LOOP_UPDATE> ))
```

The variable is initialized to the start value, updated according to the loop update, and continues until the stop condition is no longer true. The loop update (`num++`) here adds `1` to `num` each time the loop iterates. NOTE: You _don't_ precede variables with `$` inside double parentheses.

Sometimes you'll want to loop through a whole series of command-line arguments. To loop through these arguments, you can write `$@` in place of the list in your `for` statement.

```
for num in $@
do
  echo $num
done
```
