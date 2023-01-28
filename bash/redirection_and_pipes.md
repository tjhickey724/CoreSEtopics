# Ch 6 Redirection and Pipes
Bash commands get their input from stdin if called with no arguments, or from a file if one is given
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
