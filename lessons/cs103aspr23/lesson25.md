# Persistence
Web apps are primarily useful because they allow users to authenticate and store/retrieve personal information 
while also allowing some of their information to be shared with some other users.

Today we'll learn how to connect an express app to a mongodb server using the mongoose package which provides
and Object Relational Model interface to the database. 

This is a big step up from what we have done so far and requires a mastery of several new concepts and skills.
The lesson25/firstapp has been updated to interact with a database and to allow user authentication.
The goal of today's lecture is to explain how this works so that you can modify the code.

# Connecting to Mongodb
The most convenient way to develop an app using a mongodb database is to run a mongodb server on your own computer.
However, you can also create a free mongodb database in the cloud using the Atlas service.
For the purpose of this class you can use my cloud server which you can access by creating a file startup.sh
with the following contents
``` bash
export MONGODB_URI="mongodb+srv://cs103aSpr23:fJXYiKN6JyGm353v@cluster0.kgugl.mongodb.net/?retryWrites=true&w=majority"
npm start
```
and then giving the command
``` bash
bash startup.sh
```
I've put startup.sh in the .gitignore as we don't want to push this URI to the cloud where anyone could access it.

Our first step will be to have everyone pull the lastest copy of cs103a, cd into lesson25/firstapp
and startup the server! 

Then visit localhost:3000 and you will be able to signup and login and logout 

#
