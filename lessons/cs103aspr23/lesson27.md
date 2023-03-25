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

## Priority Demo
Next we show how to add a priority to the todolist demo and also to show/hide the completed items.
