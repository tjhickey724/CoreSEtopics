
## Recap - sending data from browser to server
We have seen three ways that a browser can send information to the server

### Using a form
In this case the form elements have a ```name``` attribute which the browser uses to form a JSON element to pass to the server.

The server accesses these elements with
``` javascript 
req.body.NAME
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
This is how we set (or unset) the completed flag for the todo page by linking to one of the following:
``` html
/todo?show=completed
```
or
``` html
/todo?show=todo
```
and then we can access the value passed in as show using
``` javascript
req.query.show
```

### Summary
The three access functions are
``` javascript
req.body.NAME  // to access a named element from a submitted form
req.params.NAME // to access a value passed in as part of the route path
req.query.NAME // to access a values passed as ?NAME=value on the URL
```

## Exercise
To teat your understanding you can try to add a feature to let a user edit an todo item
by sending them to a edit page with a form filled out with the current values and a submit
button to change the values.

# Authemtication
We have now seen enough Express and Mongoose to be able to read the code in ```routes/pwauth.js```
to see how username/password authentication works.

# OpenAI integratation
We can use an [openai Express API](https://github.com/openai/openai-node) to access GPT from an express app.

# API Access
We show how to access an API in Express using the [US Weather Service API](https://www.weather.gov/documentation/services-web-api) at
``` html
https://www.weather.gov/documentation/services-web-api
```
but to get the weather at an address we need to use the US Government geocoder API, e.g.
```
https://geocoding.geo.census.gov/geocoder/locations/onelineaddress?address=152%20Aspinwall%20Ave%20Brookline%20MA%2002446&benchmark=2020&format=json
```
