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

## Priority Demo
Next we show how to add a priority to the todolist demo.

* First we need to add a priority field to the todoitem model
* Then we need to modify the form so that the user can select a priority (from 1-5)
* Next we update the post route so that it reads the priority and saves it in the database
* Then we modify the list view so it includes the priority and sorts it by priority
* 
