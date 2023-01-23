


# Ch 6 Redirection and Pipes
First we review some bash commands that are frequently used together.
They all get their input from stdin if called with no arguments, or from a file if one is given
and they send their output to stdout
``` bash
cat -- sending a file to stdout
less -- view the contents of a file in the terminal and allow user to scroll up and down with arrow keys
sort -- sorting the lines of the input
uniq -- removing duplicate lines from the input
grep -- finding all lines in the input which match a pattern
wc -- counting the lines, words, and characters in the input
head -- sending the first few lines of the input to output
tail -- sending the last few lines of the input to output
tee F -- sending the input to a file F and also sending it to stdout
```

Every bash command has one input stream (stdin) and two output streams (stdout, stderr). 
These are usually the keyboard for stdin and the terminal screen for stdout and stderr, 
but Linux allows you to get the input from somewhere else and send the output somewhere else as well...
This is called redirection


## Pipes

You can send the output of one process A to be the input of another B using the pipe (A | B)
For example, to find the  20 largest files and folders in the Documents folder (or any of its subfolders)
we can use the du command (disk usage) with the -a switch (for all files in the folder or subfolders)
then pipe it into the sort command with the -n switch to sort numerically
then pipe it into the tail command with the -20 switch to send the last 20 lines to stdout
``` bash
du -a ~/Documents | sort -n | tail -20
```

## Redirection - changing the default inputs and outputs of a process

You can send the output to a file using ">"  and get the input from a file using "<"
```
du -a ~/Desktop > desktopfiles.txt
sort -nr desktopfiles.txt > sortedfiles.txt
```

You can append the output to a file using ">>"
``` bash
ls ~ > files.txt
ls Desktop >> files.txt
ls / >> files.txt
```

You can redirect standard err to a file using "2>"
For example, this will list all of the files and folders (and subfolders) in the /usr folder
but send errors to the errors.txt file (e.g. permission errors where you are not allowed to read those files or folders).
```
du -a /usr 2> errors.txt > dua.txt
```

You can redirect both stdout and stderr using "&>"

# Chapter 7 -- Argument Expansion in the shell

The features we'll discuss are specific to the BASH shell so make sure you are using the BASH shell when following along. 
Before bash runs the commands you have entered, it first expands the arguments you passed in using the rules shown below.

We can see how expansion is working using the echo command which simply prints out its arguments without further interpretation, after expansion. 
For example, the most common argument expansion operator is the wildcard *. Here are some examples.
``` bash
> echo this is a test
this is a test
> echo /*
/Applications /Library /System /Users /Volumes /bin /cores /dev /etc /home /opt /private /sbin /tmp /usr /var
> echo /*s
/Applications /Users /Volumes /cores
echo /usr/*/cups
/usr/libexec/cups /usr/share/cups
```


Here are the features:
``` bash
the * wildcard 
arithmetic $((expr))$
brace expansion: {abc,def,ghi}
brace sequences: {a..m} or {0..9} or {000..15}  or {Z..A}
parameter expansion: $PARAM
command substitution $(COMMAND)
double quoting "....."  keeps spacing but allows expansion
single quoting '....'  suppresses all expanion
escaping:  \{    \$     \\    etc.
The parameter expansion refers to environment variables which can be set in the shell using
export VARNAME=VALUE
and these are then accessed in the shell using  $VARNAME as in expansion rule 5 above...
```

# Practice
We'll give several Mastery Learning problems to give you practice with this concept



# Chapter 8 .. Command manipulation
user arrow keys and "emacs commands" to interact with the current and previous commands
``` bash
^P previous command  ^N next command (arrow keys work too)
^A beginning of command, ^E end of command, ^F move forward a character, ^B move back a character,
^K delete to the end of the line and store in the "kill buffer"   ^Y copy out the content in the "kill buffer"
^D delete the current character, "backspace" delete the previous character

```

# history expansion
The bash shell keeps a list of the most recent commands it has processed in your shell
and these can be accessed using the following commands.
``` bash
history -- displays the last several commands you've given
tab will autocomplete a command
!str  will run the most recent command in your history starting with str, e.g. !du
!! run last command
!ABC run last command which started with ABC
!34  run command with history number 34
!?ABC run last command which contained ABC
```
```



