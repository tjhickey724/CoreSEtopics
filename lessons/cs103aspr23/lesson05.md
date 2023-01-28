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



