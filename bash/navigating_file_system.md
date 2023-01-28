## Ch 2. Navigation (and The File System)
All persistent data stored in your computer is stored in files. The files on Linux systems (and almost all others) are arranged hierarchically in a structured called a "tree" with the root of the tree ("/") at the top. Each file in the tree is either a folder/directory which contains links to other files, or it is a "leaf" which could be a text file, a word document, an executable program, a movie, etc.  
Table 3.4 of TCLC shows the top level files and folder in the root directory.

The main commands for moving around the file system are:

```
pwd -- print working directory shows where you are
cd DIR -- changes to the directory DIR which can be an absolute path name (
cd /home/tim/Desktop/testing)
or relative (testing)
cd testing  
or going up to a parent
cd ..
ls -- lists the files in the current directory, there are many useful flags and you can pass arguments to list files in a specified directory
ls -al   creates a long listing of all files in the current directory
ls -alS  /user      creates a long listing of all files in the /user folder sorted by size
ls -lR .  creates a long listing of all files and folders in the current directory and the contents of all directories, recursively
```

### EXERCISES -- we get some practice using the shell
Try to connect to tiara with the command

ssh yourcsusername@tiara.cs.brandeis.edu

from terminal.app on mac or powershell on windows. Note that your user name must be all lowercase.



## Ch 3. Exploring the file system
You can use the file command to discover the type of data stored in a file. If it is a text file you can use the less command to look at the contents and scroll forward and back through the text.

``` bash
file FILEORFOLDER   returns the type of file
less TEXTFILE       allows interactive viewing of file on a terminal (use arrow keys to move up or down in the file)
```


## Ch 4 Manipulating Files and Directories
You can change the data stored in the file systems by moving files and folders around in various ways. The main commands are
``` bash
cp makes a copy of a file of any type. Copying a folder requires the "recursive" flag -r
cp OLDFILE NEWFILE   
cp -r OLDFOLDER NEWFOLDER
mv  moves one or more files to another location
mv OLDFILE NEWFILE  this renames the file
mv F1 F2 ... Fn  FOLDER  moves the folders F1, F2, ... to the specified FOLDER
mkdir  creates a new folder
mkdir NEWFOLDER
rm removes a file or folderr
rm FILE
rm -r FOLDER removes the folder and everything in it, recursively
ln  creates a link to a file or folder and gives it a name 
ln -s FILE LINK   the LINK now is a symbolic reference to the FILE, kind of like an alias or shortcut, not a copy!
```

## Ch 5. Working with commands
the following commands are helpful in learning what a command does and in making aliases
```
type, which, help, man, apropos, info, whatis, alias, 
```

