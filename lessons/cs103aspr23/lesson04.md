
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

### wildcards
``` bash
the * wildcard 
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

Changing permissions with chmod
The easiest way to set permissions is to either use an octal command
``` bash
chmod 760  
```
where the 7 refers to the user and grants them rwx (111 in octal).
the 6 refers to the group and grants the rw- (110) in octal
the 0 refers to everyone else and grants them --- (000) in octal
You can also add and remove permissions symbolically:
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

# Chapter 10: Processes
Processes are generally started by invoking a command in the shell or by running a script. Linux is a multiuser system and there are typically dozens if not hundreds of processes running simultaneously. On a multicore machines you will have several processes running at the same time, but usually the operating system has a list of processes that want to run and gives each one a short timeslice, then puts it back at the end of the list. You'll learn more about this in CS131a Operating Systems.
We can see the current processes using the ps command with options
``` bash
ps   - shows the processes you started
ps f  - shows the long form of the processes you started
ps fx  - shows which process started which!
ps af  - shows the long form of all processes
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


ssh issues solved by a flag
``` bash
ssh -oHostKeyAlgorithms=+ssh-rsa username@tiara.cs.brandeis.edu
```
