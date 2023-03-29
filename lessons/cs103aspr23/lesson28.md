## Future Homeworks
* PA03 Python SQL Transactions App was due Sunday 3/26
* PA04 NodeJS/Express Mongoose Transaction App due in 3 weeks (group or individual, your choice) due Sunday 4/16 (after Spring break)
* CA01 GPT Flask App due next Sunday 4/2  (group project)
* CA02 GPT NodeJS/Express/Mongoose App due Sunday 4/23 (individual project must store info in DB and share it)
* PA95 React App due Sunday 4/30  (group or individual project, your choice)

## Recap - sending data from browser to server
We have seen three ways that a browser can send information to the server

### Using a form
In this case the form elements have a ```name``` attribute which the browser uses to form a JSON element to pass to the server.
The server accesses these elements with
``` javascript 
req.body.NAME
```

Here is the form from the todolist example
``` html
<form method="post" action="todo">
          new item: <input type="text" name="item">
          priority: <select name="priority">
            <option>1 </option>
            <option>2 </option>
            <option>3 </option>
            <option>4 </option>
            <option>5 </option>
          </select>
          <br>
          <input type="submit" value="store">
      </form>
```
which we access in the server as follows
``` javascript
/* add the value in the body to the list associated to the key */
router.post('/todo',
  isLoggedIn,
  async (req, res, next) => {
      const todo = new ToDoItem(
        {item:req.body.item,
         createdAt: new Date(),
         completed: false,
         priority: parseInt(req.body.priority),
         userId: req.user._id
        })
      await todo.save();
      res.redirect('/todo')
});
```



### As a URL parameter
In this case, the browser sends information as one of the path elements in the route, e.g.
``` html
<a href="/todo/remove/641f193c0812e3a056050868">....</a>
```
where the id of the todo element to be edited is passed as the third path element.

We access url parameters in the router by naming it in the route path as we did for the remove action:
``` javascript
router.get('/todo/remove/:itemId',
  isLoggedIn,
  async (req, res, next) => {
      console.log("inside /todo/remove/:itemId")
      await ToDoItem.deleteOne({_id:req.params.itemId});
      res.redirect('/toDo')
});
```

### As a Query Parameter
You can also send values to the server by adding NAVE=VALUE pairs to the end of the URL
``` html
https://DOMAIN/path?NAME1=VALUE1&NAME2=VALUE2&.....&NAMEK=VALUEK
```
and we access those values in the server with the ```req.query`` object
``` javascript
req.query.NAME1   req.query.NAME2 ...
```
For example, in the todolist we can send a "show" value to the server with the following code
in view/todolist.ejs
``` html
<p class="m-2">
        <% if (show=='completed'){%>
          <a href="?show=todo">(Show tasks to do)</a>
        <% }else {%>
          <a href="?show=completed">(Show completed tasks)</a>
        <%}%>
      </p>
```


```
and then we can access the value passed in as show using
``` javascript
// get the value associated to the key
router.get('/todo/',
  isLoggedIn,
  async (req, res, next) => {
      const completed = req.query.show=='completed'
      res.locals.show = req.query.show
      res.locals.items = 
        await ToDoItem.find(
           {userId:req.user._id, completed}).sort({completed:1,priority:1,createdAt:1})
      res.render('toDoList');
});
```

### Summary
The three access functions are
``` javascript
req.body.NAME  // to access a named element from a submitted form <input .... name="...." >
req.params.NAME // to access a value passed in as part of the route path    <a href=".../<%= val._id %>">...</a>
req.query.NAME // to access a values passed as ?NAME=value on the URL  <a href=".....path?N1=V1&...Nk=Vk
```

## Exercise
To teat your understanding you can try to add a feature to let a user edit an todo item
by sending them to a edit page with a form filled out with the current values and a submit
button to change the values. We'll do this in steps...

# Authemtication
We have now seen enough Express and Mongoose to be able to read the code in ```routes/pwauth.js```
to see how username/password authentication works.

# API Access
We show how to access an API in Express using the [US Weather Service API](https://www.weather.gov/documentation/services-web-api) at
``` html
https://www.weather.gov/documentation/services-web-api
```
but to get the weather at an address we need to use the US Government geocoder API, e.g.
```
https://geocoding.geo.census.gov/geocoder/locations/onelineaddress?address=152%20Aspinwall%20Ave%20Brookline%20MA%2002446&benchmark=2020&format=json
```


# OpenAI integratation
We can use an [openai Express API](https://github.com/openai/openai-node) to access GPT from an express app.
