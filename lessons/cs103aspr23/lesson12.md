# Collaboration

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

## Practice
It is a good idea to practice some of these skills!
We can have each team pick a captain, the captain creates a repository and adds team members
The team members all clone down the team project
Then have everyone practice editing the same document and resolving conflicts..
