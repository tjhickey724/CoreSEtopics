# Chapter 10: grep and Regular Expressions

## Motivation
Searching for information is one of the key uses of computation and today we look at a very powerful tool, grep, for searching for lines in a file which match a given pattern. 

The patterns will be specified by a powerful language for creating Regular Expressions. These pattern descriptors are common throughout software development and now is a good time to learn how to use them.  

Almost every programming languages has a module to search strings for a particular regular expression (java, python, javascript, ruby, etc.)

This content comes from Chapter 19  of TLCL

you can also access the grep manual entry at this link 

https://www.gnu.org/software/grep/manual/html_node/index.html

We can test our regular expressions using this link https://regex101.com/

## Learning Objectives -- by the end of the lesson you will be able to 
explain the purpose of each of the 15 metacharacters used to create regular expressions
explain why a particular regular expression does or does not match a particular line
write regular expressions to capture particular patterns
create shell commands which look for lines matching a particular regular expression



# Activity -- grep and Regular Expressions
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

