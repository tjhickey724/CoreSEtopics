
# Chapter 7 -- Argument Expansion in the shell

The features we'll discuss are specific to the BASH shell so make sure you are using the BASH shell when following along. 
Before bash runs the commands you have entered, it first expands the arguments you passed in using the rules shown below.

We can see how expansion is working using the echo command which simply prints out its arguments without further interpretation, after expansion. 

The official documentation for bash is at the 
[gnu.org website](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html)
which has a secrtion on [argument expansion](https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html#Shell-Expansions)


### echo expands arguments and sends to stdout
``` bash
> echo this is a test
this is a test
```
Here are the main argument expansion features of bash:

### wildcards
The most common argument expansion operator is the wildcard *. Here are some examples.
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

### tilde expansion (~)
``` bash
# ~ expands to the user's home directory
echo ~/Desktop
/Users/tim/Desktop
```

### shell arithmetic
``` bash
# arithmetic $((expr))$
echo $((365*24*60))
```

### brace expansion
``` bash
# brace expansion: {abc,def,ghi}
# brace sequences: {a..m} or {0..9} or {000..15}  or {Z..A}
mkdir movies; cd movies
touch show{a,z}-{00..20}.mov
```

### parameter expansion
``` bash
# parameter expansion: $PARAM
# command substitution $(COMMAND)
echo $PATH
```

The parameter expansion refers to environment variables which can be set in the shell using
``` bash
# export VARNAME=VALUE
# and these are then accessed in the shell using  $VARNAME as in expansion rule 5 above...
export testnum=99
echo $testnum
```
You can examine the current environment varaibles with
``` bash
printenv
```

### quotation
``` bash
# double quoting "....."  keeps spacing but allows expansion
# single quoting '....'  suppresses all expanion
# escaping:  \{    \$     \\    etc. in doube quoted expressions
echo 'echo $(($testnum*2))' > test.sh
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



# Chapter 9: Permissions

The Linux operating system assigns and owner and a group to every file and uses this information to decide who gets to do what to which files.  The goal is allow multiple users to share a common disk and yet maintain privacy and allows some sharing when desired.  The Linux permission system allows you to specify permissions for
```
r - reading a file
w -writing to a file (i.e. modifiying it)
x - running a file as a program or script
You can do this separately for the
u = user
g = group (defined in /etc/groups)
o = everybody on the system
```

## viewing permissions
The long listing version of the ls command (ls -l) shows the read/write/execute permissions for the user/group/others
``` bash
bash-3.2$ ls -l ~/Desktop
total 32
drwxr-xr-x   11 tim  staff   352 Jan 19 09:49 CoreSEtopics
-rw-r--r--    1 tim  staff  8995 Jan 23 11:24 doc.txt
drwxr-xr-x    3 tim  staff    96 Jan 18 11:59 lesson01
drwxr-xr-x    5 tim  staff   160 Jan 22 13:12 lesson02
drwxr-xr-x    3 tim  staff    96 Jan 23 10:34 lesson03
drwxr-xr-x    4 tim  staff   128 Jan 24 16:14 lesson04
-rw-r--r--    1 tim  staff   437 Jan 23 11:23 ls.txt
drwxr-xr-x  128 tim  staff  4096 Jan 18 10:42 misc-20230118
```
Here is how to read a line of the long listing
* The first character in each line is d for a folder or directory.
* The next 3 are the permissions for the user, the next three for the group, and the last three for everyone else
* The next number is the size of the file or folder
* then the owner of the file
* then the group of the file
* then the size in bytes
* finally the last modified date and the file/folder name

## Changing permissions with chmod
The easiest way to set permissions is to either use an octal command
``` bash
chmod 760  
```
where the 7 refers to the user and grants them rwx (111 in octal).
the 6 refers to the group and grants the rw- (110) in octal
the 0 refers to everyone else and grants them --- (000) in octal
You can also add and remove permissions symbolically
using 
* u,g, and/or o to specify the user group or other
* and a + or - to specify adding or removing a permission
* and r,w, and/or x to specify read, write, and execute permissions

``` bash
chmod go-rwx  
```
would remove rwx from the group and others

## Changing users

Linux allows you to switch between user accounts if you know the password.
You use the su (substitute user) command with the -l switch
``` bash
su -l USERNAME -- switches to another user account
```

There is a distinguished user on a Linux system called the superuser that owns the process that starts the entire system. You are generally not allowed to use su to be come the superuser, but if you are on the list of "sudoers"  (/etc/sudoers) you can run a command as superuser using sudo

``` bash
sudo COMMANDS -- runs the commands as the superuser
```

If you own a file, you can change the owner and change the group using the chown and chgrp commands.  You can also change the password using the passwd command.



ssh issues solved by a flag
``` bash
ssh -oHostKeyAlgorithms=+ssh-rsa username@tiara.cs.brandeis.edu
```
