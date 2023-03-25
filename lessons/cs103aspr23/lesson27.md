## Bootstrap Icons
The [Bootstrap Icon library](https://icons.getbootstrap.com/)
has over 1800 icons. The easiest way to access them, in my opinion,
is to add the icon css link into the head of your pages (in the layout.ejs file)

``` html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.3/font/bootstrap-icons.css">
```

and then when you click on an incon in the page listed above, it shows you the code you need to insert.
For example to insert a trashcan we use
``` html
<i class="bi bi-trash"></i>
```
where "bi" stands for Bootstrap Icon.

## Spacing
Bootstrap also has nice model for adding padding and margins to the top, botton, left, and right of an element
This is the [spacing model](https://getbootstrap.com/docs/5.3/utilities/spacing/#gap)
For example to add a level 5 margin to both left and right of an element use
``` html
class="mx-5"
```
We've used icons and spacing to improve the todolist feature and make it look a littl emore professional.

## Showing/Hiding Completed Items
We show how to modify the todo app to allow users to hide or view the completed tasks.
We do this using query parameters, so rather than just linking to ```/todo```
we have a link to 
``` html
/todo?show=completed
```
or
``` html
/todo?show=todo
```
These are called query arguments (and we can additional ones separated by ampersands).
To read these in the router we use
``` javascript
const completed = req.query.show=='completed'
```
where express uses req.query.NAME to refer to the query argument with the specified NAME.

We can then use this query argument to modify the ".find" method in the router 
``` javascript
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
so ```req.query.show``` gets the query parameter (either completed or todo)
and we send that value to the view as ```res.locals.show```
and we use it to lookup the users todo items, either completed or not completed.
Finally, we sort the items hy completed first, then priority, the by creation date.

## Priority Demo
Next we show how to add a priority to the todolist demo.

1. First we need to add a priority field to the todoitem model in ```models/TodoItem.js```
1. Then we need to modify the form in ```views/toDoList.ejs``` so that the user can select a priority (from 1-5)
1. Next we update the post route in ```routes/todo.js``` so that it reads the priority and saves it in the database
1. Then we modify the list view int ```views/toDoList.ejs``` so it includes the priority and sorts it by priority

We will do these in class and have you do them to gain experience in modifying the app.



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

