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


