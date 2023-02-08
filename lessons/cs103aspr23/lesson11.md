# Github and using GIT for team collaboration
Today we begin by talking about how to use git in a team

## Github
Github is one of the most well-known git repository servers.  Everyone should register for a git account
so you can practice using git. You'll use it in a team project soon. Visit this site to register
``` bash
https://github.com
```

## Cloning a repository
The simplest way to connect to a git repository on github is to clone an existing repository.
You can create an empty repository on github and the clone a copy onto your computer.
You can also clone an existing repository as long as it is public. You will only be allowed to push changes
into it if you have been set as a collaborator on that project, but you can always pull down the latest changes
that others have made. If your username is USER and your repository name is REPO then clone command is
``` bash
git clone https://github.com/USER/REPO.git
```

## Pushing a repository into github
If you already have a folder you want to push into github, you do the following
1. initialize the repository with ```git init .``` as above
2. create a ```.gitignore``` specifying which files you do not want pushed into the repository
3. add the files to the repository with ```git add .```
4. commit those files to the repository with ```git commit -m "first commit"```
5. create a repository REPO on github
6. set the REPO as a remote repository for your folder with ```git remote add origin https://github.com/USER/REPO.git```
7. pull the repository to your folder with ```git pull origin main```
8. push your changes to the cloud with ```git push origin main```

## Syncing with a remote
If you have cloned your repository from the cloud, then you can sync it using
``` bash
git push origin main
```
where origin is the name of the remote repository and main is the name of the current branch. If someone else has pushed a change to the repository, then git won't let you push. You'll first have to pull down their changes.
``` bash
git pull origin main
```


## Resolving Conflicts
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

It is a good idea to practice some of these skills!
