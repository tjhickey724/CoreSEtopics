# Lesson 05

## Scripting
We can create our own commands by putting them in a file with a special first line(#!/bin/bash)
and then we can run it using bash or we can change its permission to make it executable and run it by name.

For example, let's make a hello world script

``` bash
#!/bin/bash
echo "Hello World"
echo 'Here are the commands in /bin how many do you know?'
ls /bin
echo 'Goodbye'
```

If we put this in a file called p1.sh we can run it with the command
``` bash
bash p1.sh
```

We might however want to make it a command we can just run by giving its name, like ls or cd, to do this we have to change its permission to make it executable, and we usually get rid of the .sh suffix
``` bash
cp p1.sh p1
chmod p1 700
```
We can then run it using
``` bash
./p1
```
But why the "./" at the beginning? To see that we have to know about the PATH variable
The PATH 
Bash has a special environment variable called PATH. You can get its value using
``` bash
echo $PATH
```
or see all of the environment variables with
``` bash
printenv
```
When the bash shell is running a command, it uses the path to look for the code for that command. The code could be either low level machine code, or it could be a script as we have written.  By default the current directory ('.') is not on the path so it won't look for "p1" in our directory.
We can change that by adding "." to the path:
``` bash
bash
echo $PATH
export PATH=$PATH:'.'
echo $PATH
```
running these commands we see that we have added '.' to the end of the path so it will look there last. This could be dangerous and we don't usually do it.

## .bashrc and the path
A better approach is to create a folder in your home folder to store your scripts and add it to your path.

You can modify your path in the .bashrc file which will get evaluated when you start up the shell.
For example, if you have some programs you've written and want them to be run as commands, you can put them
in the file ~/pyscripts and add this to your path as follows. Each time you start the bash shell it will update the path
to include ~/myscripts
``` bash
# .bashrc
export PATH="$PATH:$HOME/myscripts"
```

## command arguments in bash scripts
You can use the following special variables in a bash script
``` bash
$# the number of arguments
$0 the name of the command
$1 the first argument
$2 the second argument, etc
```
For example, if we create the following file called myssh and change its permissions ot 700
``` bash
#!/bin/bash
ssh  -oHostKeyAlgorithms=+ssh-rsa $1@$2.cs.brandeis.edu
```
then we can ssh into any cs machine by any user as follows:
``` bash
./myssh tim tiara
```
and if we put myssh into ~/myscripts you can run it just as 
``` bash
myssh tim tiara
```

# Chapter 10: Processes
Processes are generally started by invoking a command in the shell or by running a script. Linux is a multiuser system and there are typically dozens if not hundreds of processes running simultaneously. On a multicore machines you will have several processes running at the same time, but usually the operating system has a list of processes that want to run and gives each one a short timeslice, then puts it back at the end of the list. You'll learn more about this in CS131a Operating Systems.
We can see the current processes using the ps command with options, use the man ps command to learn more commands
``` bash
ps   - shows the processes you started
ps -ajv shows more info about each process
```

We can get a graphical view that is continually updated using the top command
``` bash
top
```
and you can give commands to change the view
``` bash
h - brings up the help menu
q - quits the program
u tim  - show the processes owned by tim
```
Sometimes a process will take a long time to run and Linux allows you to pause it using control-Z
you can then  restart it with the fg command or run it in the background with the bg command. You can also start the job running in the background immediately using the & command:
``` bash
du -a &> dua-20220126.txt &
```

Note that the &> is sending both stdout and stderr to the file, while the & at the end is running the process in the background.
``` bash
jobs -- show all of the jobs running in your shell and gives them numbers, 1,2,3,...
^Z -- pause the current job
^C -- kill the current job
bg %N-- run the currently paused job in the background
fg %N -- bring the background job to the foreground
```

# Chapter 10: grep and Regular Expressions

## Motivation
Searching for information is one of the key uses of computation and today we look at a very powerful tool, grep, for searching for lines in a file which match a given pattern. 

The patterns will be specified by a powerful language for creating Regular Expressions. These pattern descriptors are common throughout software development and now is a good time to learn how to use them.  

Almost every programming languages has a module to search strings for a particular regular expression (java, python, javascript, ruby, etc.)

This content comes from Chapter 19  of TLCL

you can also access the grep manual entry at this link 

https://www.gnu.org/software/grep/manual/html_node/index.html

## Learning Objectives -- by the end of the lesson you will be able to 
explain the purpose of each of the 15 metacharacters used to create regular expressions
explain why a particular regular expression does or does not match a particular line
write regular expressions to capture particular patterns
create shell commands which look for lines matching a particular regular expression



#Activity -- grep and Regular Expressions
You should read Chapter 19 in TLCL to review the content we cover today (it should have been on the homework 2 reading list...)
Regular Expressions are strings of characters representing a particular kind of pattern. There are 15 characters that have special meaning, these are the so-called metacharacters:
``` bash
^  $  .  [  ] { }  - ? * + ( )  | \
```
If you want to include a metacharacter as a simple character you need to "escape it" by putting a backslash before it.

We will talk about two ways of looking for a pattern R in a file F.
``` bash
grep R F
egrep R F which is the same as grep -E R F
```
the second is for extended regular expressions ...

## Simple Patterns
The simplest patterns are just sequences of characters, e.g.
``` bash
ls /usr/bin | grep 'zip'
```
will find all of the files in /usr/bin which have zip in their name.

We can specify that zip has to appear at the beginning of a line using ^
``` bash
ls /usr/bin | grep '^zip'
```
and at the end by using $
``` bash
ls /usr/bin | grep 'zip$'
```
We can also have patterns mixing particular characters and "any" characters represented by '.'
``` bash
ls /usr/bin | grep 's.f'
```
This can be useful for crossword puzzles (or wordle!) using the file /usr/share/dict/words of English words, e.g.
``` bash
grep '^q...n$' /usr/share/dict/words
```
Notice that we use single quotes around the pattern so that the shell doesn't try to expand it when it sees special characters like $ and {} etc.

## Bracket Expressions
Regular expressions allow you to include options in a pattern using the square brackets
``` bash
ls /usr/bin | grep '^[bg]zip'
```
and we can use ^ at the beginning of a bracket expression to represent negation
``` bash
ls /usr/bin | grep '^[^aeiou]..$'
```
this finds all commands with 3 letters that don't start with a vowel!

## Character Classes
We can use the dash to get character ranges, e.g. 
``` bash
[A-Z] [a-z] ]0-9]
```

and there are several builtin character classes
``` bash
[:alnum:]   same as {A-Za-z0-9]
[:punct:] the punctuation symbols
[:space:]  white space ... also you can use \s
```
you can see a full list in the book page 260 or do a google search!

https://www.gnu.org/software/grep/manual/html_node/Character-Classes-and-Bracket-Expressions.html
https://www.gnu.org/software/grep/manual/html_node/The-Backslash-Character-and-Special-Expressions.html

## Extended Regular Expressions
Basic regular expressions use six metacharacters  
``` bash
^ $ . [ ] *
```

but Extended also includes 
``` bash
(){}?|+| 
```
which we discuss below

## Alternation with |
Disjunctive patterns (this or that or the other) are specified with a vertical bar  and we need to use the -E flag to specify extended Regular Expressions. So for example, the following will search for all files that start with bz, gz, or zip. We can also use parenthesis to combine the alternation with other components (such as the ^ anchor for the beginning of a line.
``` bash
ls /usr/bin | grep -E '^(bz|gz|zip)'
```

## Quantifiers and Repetition with ? , +, and {n,m}
We can specify how many times a pattern should repeat using 
``` bash
E?  if the expression should appear zero or one time, i.e. is optional
E+ if the expression should repeat one or more times
E* if the expression should repeat zero or more times
E{n,m} if the number of times, t, the expression repeats satisfies n<=t<=m
E{n,}  it repeats at least n times
E{,m} it repeats at most m times (possibly none)
E{n} it repeats exactly n times
```
For example we could look for a phone number pattern (ddd)-ddd-dddd in a file phones.txt with
``` bash
grep -E  "\([0-9]{3}\)-[0-9]{3}-[0-9]{4}" phonelist.txt 
```
We can create the phonelist with this shell script:
``` bash
for i in {1..10}; do echo "(${RANDOM:0:3})-${RANDOM:0:3}-${RANDOM:0:4}" >> phonelist.txt; done
```
You can learn about Advanced Shell Scripts in Part 4 of TLCL but we will skip it for now.

We can find the lines that don't fit the pattern using the -v option for grep:
``` bash
grep -E  -v "\([0-9]{3}\)-[0-9]{3}-[0-9]{4}" phonelist.txt 
```
as the RANDOM function will sometime generate fewer digits than expected.



