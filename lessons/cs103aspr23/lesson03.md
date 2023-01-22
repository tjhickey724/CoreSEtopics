


# Ch 6 Redirection and Pipes
First we review some bash commands that are frequently used together
``` bash
cat, sort, uniq, grep, wc, head, tail, tee
```

Every bash command has one input stream (stdin) and two output streams (stdout, stderr). 
These are usually the keyboard for stdin and the terminal screen for stdout and stderr, 
but Linux allows you to get the input from somewhere else and send the output somewhere else as well...
This is called redirection

## Redirection - y

You can send the output to a file using ">"  and get the input from a file using "<"

You can append the output to a file using ">>"

You can send the output to another process (as it's stdin) using "|"

You can redirect standard err to a file using "2>"

You can redirect both stdout and stderr using "&>"

## Pipes

You can send the output of one process A to be the input of another B using the pipe (A | B)

