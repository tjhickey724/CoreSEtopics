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


