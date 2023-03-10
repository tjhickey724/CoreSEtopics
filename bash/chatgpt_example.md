# Using ChatGPT to generate Bash code
If we give the following request to chatgpt
```txt
Write a bash script with two parameters. 
It should change directory to the first parameter 
and then list all files that end with the second parameter.
```
It responds (correctly) as follows (or it did on 1/5/2023):
***
Here's a bash script that does that:
```bash
#!/bin/bash
# Check if both arguments are provided
if [ $# -ne 2 ]; then
  echo "Error: Two arguments required"
  exit 1
fi
# Change to the directory specified by the first argument
cd "$1"
# List all files that end in the string specified by the second argument
ls -p | grep -v / | grep "$2$"
```

To use this script, save it to a file and make it executable with the chmod command:
```bash
chmod +x script.sh
```
Then you can run it by providing the two arguments:
```bash
./script.sh /path/to/directory file_ending
```
This script will list all files in the specified directory that end with the string specified by the second argument. Note that it will not list directories, only files.
***

# Activity -- Try it out!
Let's try out this script by cutting/pasting it into a file called script.sh and then running the following commands
```bash
chmod +x script.sh
./script.sh /bin s
```
This should print the following two lines:
```bash
ls
ps
```
which are the two files in the folder "/bin" which end in "s"

# Activity -- Script Analysis
We look over this script produced by chatGPT and use it to introduce various features of the shell and shell scripting.

1. The shebang line  #!/bin/bash  tells the shell which program to use to run this script. In this case we are using the bash shell (located at /bin/bash on most linux systems). But we could also have it run Python code, or Java code, or any Rust code!

2. Comments are indicated using the hashtag symbol # and the shell processor ignores the rest of the line.

3. shell control structures -- you can include conditionals with the if statement. We won't go into the details of the syntax but you can look it up using google or asking chatGPT
"how to you use conditional execution in bash scripts"
Note that you can't trust either answer and need to validate it my seeking extra references and by trying it out yourself! In this case, the "$#" symbol evaluates as the number of parameters.  The echo command simply sends its argument to the output (stdout), and the exit command stops the script with the value 1, indicating the script failed, (0 would indicate it succeeded and is the default).

4. Arguments.  When you call a shell command, the strings that appear after the command are called arguments are and accessible in the script as $1, $2, $3, ... for the 1st, 2nd, 3rd argument etc.   This is a little nuanced because the shell will do some preprocessing on the arguments as we'll see later but you can make sure that only strings are passed by putting the values in quotes "value"
The "cd $1" command changes directory to the folder specified in the first argument to the command.

5. Pipes.  The last line of the script is as follows:
```bash
ls -p | grep -v / | grep "$2$"
```
it consists of three commands, one ls and two grep where the output of one is sent as the input of the next. This is called piping.
* the results of the first command "ls -p" is sent as input to the next command, it consists of the names of files and folders, one per line, where each folder has a "/" at the end of its name
* "grep -v /", which removes all of the files containing a "/", and whose output is then sent to the last command
* "grep "$2$", which filters out only those lines containing the second argument $2, at the end of the line "$"

The "ls -p" command lists all of the files and folders in the working directory and puts a "/" at the end of each folder (but not after a file). The results are sent with one file or folder per line.

The "grep -v /" command is a pattern matcher. With the "-v" switch it returns all lines that don't contain the pattern "/"

The "grep "$2$" command returns all lines which contain the second argument $2" at the end of the line which is indicated by the final "$" in the pattern.

We will look in more detail at the grep command next week. It is a pattern matching application which is very powerful and well worth knowing!



