# A Minimal Express app
We first take a tour of a minimal express app with only four files
* app.js
* package.json
* views/index.ejs
* views/error.ejs

This lacks some of the functionality and error handling of the app
created by calling "express" but it is also easier to understand.


# Styling a web app
Today we begin with learning how to style a web app using 
[Twitter Bootstrap](https://getbootstrap.com/)
which allows us to add style to a webapp using the class attribute.

and
[Express-ejs-layouts](https://www.npmjs.com/package/express-ejs-layouts)
which allows us to define the common parts of every web page rather than
repeating it in every ejs file.

## Bootstrap
Bootstrap is a CSS and Javascript library which provides style to a webpage.
We'l look at the [documentation]([https://getbootstrap.com/docs/5.3](https://getbootstrap.com/docs/5.3/getting-started/introduction/))
and will show how to use
* the row/col grid feature
* table styling
* colors
* navbar components



# Persistence
If we have time, we learn how to connect a mongo database to an express webapp. 
This will allow us to create signup/login pages for authentication
and to store and recall personal information on the webapp.

We could use SQL to do this, but in this class we will use a NOSQL database callend Mongodb
and we will use an Object Relational Model, called Mongoose.

The first step is to download and install the
[free community mongodb server](https://www.mongodb.com/try/download/community)

You need to create a folder to store your database files,e.g.
``` bash
mkdir ~/data
```
Then you can start the mongodb server with
``` bash
mongod --dbpath ~/data
```
and you can start the mongo shell with
``` bash
mongo
```
It is also a good idea to download and install the compass app

We will be using the [Mongoose ORM](https://mongoosejs.com/docs/)
to hae our express app interact with the database.
