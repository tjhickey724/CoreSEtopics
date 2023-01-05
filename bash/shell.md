# Core Shell Concepts

Modern computer systems (e.g. your laptop or desktop computer) consist of a few primary components:
1. disk memory for storing programs and data
2. processors for running programs (which may read and write data in the disk memory
3. internet connections for sending and receiving data with other computers 
4. ports for connecting to other devices (which can send and receive data)

## The File System
The disk memory is organized hierarchically into a file system,
which consists of a toplevel folder called the root (and accessed as '/')
Each folder in the file system may contain subfolders, data files, or applications that can be run.

Each file and folder has a name consisting the full path from the root "/" to the file separated by "/" for mac/linux and "\" for windows command shell.
For example on the Mac, 
* / is the root folder
* /Users is the folder containing all user folders
* /Applications is the folder containing applications
* /bin is the folder containing core shell commands
* /bin/ls is the application for listing files
* /usr/local/bin/ is the folder containing applications installed by the user

## File and Folder Permissions
Each file or folders has some metadata specifying who has permissions to view, run, or momdify the file.
This allows multiple users to use the same computer system while still maintaining privacy (if they want it).
Each user, when they log in, has a home directory with permissions that allow them to read, create, and execute any file.
The permissions are usually set to not allow other users to access the user's files and folders at all, but this can be changed.

## The Shell 
The shell is a program which responds to user interaction on the keyboard.
The shell keeps track of a few things:
1. the current location in the file system (called the working directory)
2. the environment variables: a set of variables whose values can be accessed by user commands
3. the current user

One important environment variables it the PATH variable which contains a list of all folders in the tree
where the shell will look for applications to run when the user types in commands.

Most modern computer systems have several different shells, which are fairly similar in terms of what they let you do, but may have different command names and different syntax. For example, on MacOS you have the default shell (zsh) that is run when you run terminal.app, but you can also run the bash shell and several others (sh, ksh, dash, csh, tcsh). On windows you have the commannd prompt and the powershell, and if you install the Windows Subsytem for Linux, you can access other standard shells (like, bash, csh, etc.)

## Core commands in bash and powershell
The following are the basic commands for navigating the filesystem which are found in most of the modern unix based shells

* pwd == print working directory
* ls  == list the files in the current directory
* cd FOLDER == change directory to the specified folder (we use .. for the parent folder, and "/" to navigate to subfolders)
* cd == return to the home directory
* mkdir FOLDER == create a new folder with the specified name
* touch FILE == create an empty file with the specifed name
* rm FILE == remove the specified file
* rmdir FOLDER = remove the specified folder (which must be empty)
* ./APPNAME == run the specified file as an application


## Applications
In the shell you can invoke programs by name. The shell consults the $PATH environent variable and searches the folders on that list until it finds an executable application with name, at which time it runs that applicat6ion.  If none is found, then it throws an error.

Executable applications take their input from two places:
1. the arguments, e.g. "ls -l ~/Downloads" sends the strings "-l" and "~/Downloads" as arguments to the application in /bin/ls
2. the input stream, stdin.  You can send input into an application using the "< file" arguments, e.g. grep "abc" < test.txt will send the file test.txt (if it exists) to the application "grep" called with arguments "abc"

Executable applications send their output to two places:
1. stdout is a stream of data representing the expected output
2. stderr is a stream of data representing error messages

We will see later how you can use the shell to specify where these input streams should come from and where the output streams should go!


