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

## connecting to my cloud database
For the purpose of this class you can use my cloud server which you can access by creating a file startup.sh
with the following contents
``` bash
export MONGODB_URI="mongodb+srv://cs103aSpr23:PASSWORDv@cluster0.kgugl.mongodb.net/?retryWrites=true&w=majority"
npm start
```
where the PASSWORD is on our Latte page...
and then giving the command
``` bash
bash startup.sh
```
I've put startup.sh in the .gitignore as we don't want to push this URI to the cloud where anyone could access it.


Our first step will be to have everyone pull the lastest copy of cs103a, cd into lesson25/firstapp
and startup the server! 

Then visit localhost:3000 and you will be able to signup and login and logout 
You can only see the about page if you login, 
and if you try to visit it without being logged in, you'll be redirected to the login/signup page

# The User Model
Since we need to store user info in the cloud, we will create a schema (like a SQL table) for users.
This is done in the file models/User.js whose content is below:
``` javascript

'use strict';
const mongoose = require( 'mongoose' );
const Schema = mongoose.Schema;

var userSchema = Schema( {
  username:String,
  passphrase: String,
  age:Number,
} );

module.exports = mongoose.model( 'User', userSchema );
```
We will create similar schema for other tables of data we may need in the future (e.g. for a todo list or a transaction list).
If you download the Compass App you can look into the database and see how the data is stored.

# Modifications to app.js
Next we look at the modifications we need to make to app.js to add a database and user authentication

## loading in the User model
``` javascript
9,42d8
< 
< const User = require('./models/User')
< 
```

## Connecting to the database
``` javascript
< const mongodb_URI = process.env.MONGODB_URI || 'mongodb://127.0.0.1:27017/pwdemo';
< console.log('MONGODB_URI=',process.env.MONGODB_URI);

< const mongoose = require( 'mongoose' );
< 
< mongoose.connect( mongodb_URI);
< 
< const db = mongoose.connection;
< 
< 
< db.on('error', console.error.bind(console, 'connection error:'));
< db.once('open', function() {
<   console.log("we are connected!!!")
< });
```

## creating the signup/login/logout routes

``` javascript
< 
< const pw_auth_router = require('./routes/pwauth')
```

## enabling session handling
So the user doesn't have to give their username/password for every page they want to see
``` javascript
< 
< const session = require("express-session"); // to handle sessions using cookies 
< var MongoDBStore = require('connect-mongodb-session')(session);
< 
< const store = new MongoDBStore({
<   uri: mongodb_URI,
<   collection: 'mySessions'
< });
< 
< // Catch errors                                                                      
< store.on('error', function(error) {
<   console.log(error);
< });
< 
```

## creating middleware to require the user to be logged in
``` javascript
/* **************************************** */
/*  middleware to make sure a user is logged in */
/* **************************************** */
function isLoggedIn(req, res, next) {
  "if they are logged in, continue; otherwise redirect to /login "
  if (res.locals.loggedIn) {
    next();
  } else {
    res.redirect("/login");
  }
}
```

## adding the session handler middleware to the router
``` javascript
< app.use(session({
<   secret: 'This is a secret',
<   cookie: {
<     maxAge: 1000 * 60 * 60 * 24 * 7 // 1 week                                        
<   },
<   store: store,
<   // Boilerplate options, see:                                                       
<   // * https://www.npmjs.com/package/express-session#resave                          
<   // * https://www.npmjs.com/package/express-session#saveuninitialized               
<   resave: true,
<   saveUninitialized: true
< }));
< 
```

## adding the password authentication router middleware to the router
``` javascript
< 
< app.use(pw_auth_router)
< app.use(layouts);
< 
78,83d25
< 
```
## adding the about route
``` javascript
< app.get('/about', 
<  isLoggedIn,
<  (req,res,next) => {
<    res.render('about');
<  }
< )
< 
< 
```

## updating the layout
We also need to add login/logout links to the layout by modifing the layout as follows.
This uses the loggedIn boolean (from res.locals.loggedIn) to decide whether to draw a login button
or a logout button with an about page link. Otherwise the code is essentially the same as last time.
``` javascript

    <nav class="navbar navbar-expand-lg bg-body-tertiary">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">CS103aSpr23</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <a class="nav-link active" aria-current="page" href="/">Home</a>
            </li>
            

            <% if (!loggedIn) {%>
              <li class="nav-item">
                <a class="nav-link" href="/login">
                  <span class="button">Login</span>
                </a>
              </li>
              
              <% } else  { %>
                <li class="nav-item">
                  <a class="nav-link" href="about">About</a>
                </li>

                <li class="nav-item">
                  <a class="nav-link" href="/logout">
                    <span class="button">Logout</span>
                  </a>
              ``</li>

                <li class="nav-item">
                  <a class="nav-link" href="#">
                    <%= user.username %>
                  </a>
                </li>
              <% } %>
 
          </ul>
         
        </div>
      </div>
    </nav>

``1


## Password Authentication
We look over the code in routes/pw_auth.js to explain how the authentication router works.


# Implementing a todolist feature in our app
We now show how to implement a todolist feature.
We'll need to 
* create a form to add new todo's
* create a view to show all of the todo's which are not completed
* create a model to store the todo info
* create routes to show redner the form 
* create a route to proess the info from the form

We'll work through this and have you implement it step by step 
and add it to the current app.
