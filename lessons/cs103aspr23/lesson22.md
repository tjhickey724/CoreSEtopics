# Web App Development with NodeJS

Today we begin our introduction to full-stack development,
which means developing the code for a webapp including
- front-end code generating webpages the user interacts with
- back-end code that processes data submitted by the users
- database code, that stores user data in a database and retrieves it to use in new webpages

## Installing Nodejs
We will assume everyone has installed nodeJs following the instructions at this website:
https://nodejs.org/en/

## Learning Objectives -- by the end of the lesson you will be able to
- add new routes to an express app
- use EJS to create a page with the standard HTML components
- use the Bootstrap row/col system to layout a page of HTML components
- create a form which solicits information from the user

## Creating our first app
We begin by installing the express generator and creating a new express app from scratch
``` bash
npm install -g express-generator
cd Desktop
express --ejs --view ejs myapp
cd myapp
npm install
npm start
```
This creates a minimal app that we will modify in class today.
Next we introduce some of the core HTML and CSS features and get some practice using them!

## Tour of the App
We walk through the basic app and explain the Model/View/Controller structure of the server
and explain how app.js is best thought of as a "server generator"
``` bash
app.js
bin/
package-lock.json
package.json
public/
routes/
views/
```
and the folders have the following contents
``` bash

firstapp/bin:
  www

firstapp/public/images: EMPTY

firstapp/public/javascripts: EMPTY

firstapp/public/stylesheets: 
  style.css

firstapp/routes:
  index.js
  users.js

firstapp/views:
  error.ejs
  index.ejs
```

## HTML
We begin by looking at the standard HTML components to create paragraphs, images, links, lists, and tables, and we create several new pages with links from the main page.
https://developer.mozilla.org/en-US/docs/Web/HTML

## Forms
Next we look at creating forms that contain buttons, dropdown menus, and other input elements

## Layout
Next we look at boostrap layout using the row/col grid system
https://getbootstrap.com/

## CSS
We give a brief intro to CSS
https://developer.mozilla.org/en-US/docs/Web/CSS
http://www.csszengarden.com/



