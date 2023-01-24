
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

ssh issues solved by a flag
ssh -oHostKeyAlgorithms=+ssh-rsa username@tiara.cs.brandeis.edu

