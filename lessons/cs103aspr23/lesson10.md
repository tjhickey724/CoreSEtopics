# Collaborative Software Development

## Activity -- GIT
We have everyone download the latest version of git, if they don't already have it downloaded.
https://git-scm.com/downloads
I will demonstrate the main features of git when used for collaboration, by selecting some volunteers and creating a github repository with multiple branches.  For Wednesday, I'll ask everyone to join a team of 3-5 students for completing the first programming assignment: PA01 and we'll do some more practice in class of setting up a repository, adding collaborators, and using it to develop a software project.

Creating a git repository.
There are two ways to create a git repository, either by initializing an existing repository
``` bash
git init .
```
or by cloning a repository from the cloud (usually from github)
``` bash
git clone URL.git
```
In both approaches  the folder will have a hidden subfolder (.git) which contains all of the previous versions of the folder in a compressed form. It may also contain all previous versions of different branches of the folder.
We also usually create a file (.gitignore) in the folder which tells git which file to NOT put in the repository. For example, the files named .DS_Store are Mac only files which don't need to be there so the .gitignore can have
``` bash
.DS_Store
```
along with any other files or folder you don't want in the repository. You can use the * wild card to hide patterns of files and folders (e.g. *.class or *.o or *~)

When we create a repository from a folder on our computer we add the files in the folder to the repository (except those in ``` .gitignore```) using
``` bash
git add .
```
Four status levels of a file.
There are four different status levels for any file or folder in a repository

* untracked (e.g. if it is in the .gitignore__
* unmodified and tracked 
* modified and tracked
* staged

the ```git add FILE``` operation moves modifed files into the staging area, but doesn't put them in the permanent repository. 
To do that we have to commit using
``` bash
git commit -m "first commit or some other messsage"
```
If you forget the -m message you will be thrown into an editor (typically vi) and you'll need to exit using
``` bash
ESC : q!
```
where ESC means pressing the Escape key (esc)

Note that if you stage a file using git add but then modify it, only the staged version will go into the permanent repository, you'll need to add it again.

To see the status of all files in your repository use the git status command
``` bash
git status
```
which shows which file are modified (and tracked or untracked) and which are staged.

## Creating a Github Repository
We show how to create a github repository. There are two common approaches.
Create an empty repository on github and clone it down to your computer and then start the project.
Create an empty repository on github and add it as a remote to your local repository
The second is more common and we'll do that one.

## Git Practice
We will have everyone create a git repository for their notes for this class
cs103aNotes
and we'll use it to store any code we write in class. This way you will have access to it even if your disk crashes or you want to work on a different machine. 



## Syncing with a remote
If you have cloned your repository from the cloud, then you can sync it using
``` bash
git push origin main
```
where origin is the name of the remote repository and main is the name of the current branch. If someone else has pushed a change to the repository, then git won't let you push. You'll first have to pull down their changes.
``` bash
git pull origin main
```


Resolving Conflicts
If their changes conflict with your changes (e.g. you both changed the same line of the same file), then git will mark those conflicts with the following syntax
```
<<<<<<<<<
your code
=========
their code
>>>>>>>>>
```

so  you'll need to search through the files to find the "<<<<<" and replace that whole section with what you want the code to be.  This is called Resolving a Conflict. 

You can find the files which contain conflicts (and the line numbers) using
``` bash
git diff --check
```

## Branching
In large projects the team typically manages several different branches. At the least there will be

the production branch which is the official branch that the customers are currently using, and
the development branch, which is where the new features are being developed and tested
It is typical for individuals to also maintain temporary branches where they tryout their changes, run the battery of tests to make sure they didn't break anything, and then merge their change to the development branch. For larger projects, the team members all clone the main project and the create pull requests which are reviewed by the team leaders before being merged into the development branch. We'll get into that later.

You can create a new branch using
``` bash
git checkout -b devel
```
and you can see all of the branches (and which one you are on) using
``` bash
git branch
```
You can switch between branches using checkout
``` bash
git checkout main
```
and you can merge the devel branch into the main branch, using merge
``` bash
git checkout main
get merge devel
```
This may create conflicts which you will need to resolve before you can commit and push to origin.
