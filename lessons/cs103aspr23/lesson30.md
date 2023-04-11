# Aggregation and PA04

## Future Homeworks
* PA03 Python SQL Transactions App was due Sunday 3/26
* CA01 GPT Flask App was due Sunday 4/2 (group project)
* PA04 NodeJS/Express Mongoose Transaction App due 4/23 (individual project)
* CA02 GPT NodeJS/Express/Mongoose App due Sunday 4/30 (individual project must store info in DB and share it)
* PA05 React App due Wednesday 5/3 (optional individual project for extra credit)

## PA04
PA04 is available now and due a week after we get back from break.
You are to implement essentially the same Transaction App, but as an Express webapp
with Mongodb instead of as a Python app with SQL. You will need to do some aggregation
for this project, so we'll a talk about that today.


## Intro to Aggregation

We show how to do groupby and pivot tables using mongoose via these slides:
https://docs.google.com/presentation/d/1x8fbBwss3XEG3FOs1aTz8-WhIsgmBd1xFL60K9DMxn0/edit?usp=sharing


You may also want to look over this tutorial:
https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/



## Useful links:
Aggregation Quick Reference:
https://www.mongodb.com/docs/manual/meta/aggregation-quick-reference/

### Particular Stages:
$project  produces new stream of documents with only the specified fields (see also $addfields)

$group  produces a new stream of document which contain a summary of the original stream
   this requires you to specify the fields to be grouped by (using the _id field)
   and the accumulation operator with an expression to accumulate

$match produces a new stream by filtering the current stream in the same way the Model.find operator works

$sort reorders the stream based on a list of fields that are specified to increase or decrease

## Adding aggregation to the ToDoList
Let's use aggregation to find the number of todos on each users todolist!

To do this we first make a slight change to the ToDoList model which will allow us to easily get a user's username from their userId. The change is to add a "ref" field for the userId so Mongoose will be able to determine which Collection that userId refers to.
``` javascript

'use strict';
const mongoose = require( 'mongoose' );
const Schema = mongoose.Schema;
const ObjectId = mongoose.Schema.Types.ObjectId;

var toDoItemSchema = Schema( {
  item: String,
  completed: Boolean,
  createdAt: Date,
  priority: Number,
  userId: {type:ObjectId, ref:'user' }
} );

module.exports = mongoose.model( 'ToDoItem', toDoItemSchema );

```
Next we add an route using the aggregate method.
In this case we are counting the number of ToDoItems
that each user has created. Then we are sorting them
by those numbers.
Finally, we are using the "userId" field in the results
to lookup the actual user data (but only the username and age).
We'll explore this more closely using the res.json calls instead
of res.render to see what the aggregation produces.
``` javascript
router.get('/todo/byUser',
  isLoggedIn,
  async (req, res, next) => {
      let results =
            await ToDoItem.aggregate(
                [ 
                  {$group:{
                    _id:'$userId',
                    total:{$count:{}}
                    }},
                  {$sort:{total:-1}},              
                ])
              
        results = 
           await User.populate(results,
                   {path:'_id',
                   select:['username','age']})

        //res.json(results)
        res.render('summarizeByUser',{results})
});
```

Finally, we put this data into an HTML table.
We have discussed them yet, but the table is an HTML element
which contains a thead element (for the headers) and a tbody element
for the rows of data.
Each row is represented by a <tr> element and the columns by <td> elements.
   
As usual we use the map method to iterate through the items, and
use each item to generate a row (tr) with the username, age, and number of todos
as columns (td)
   
``` javascript
<h1>Summary of all ToDoItem Users</h1>
<table class="table table-bordered table-striped">
    <thead>
        <tr>
            <th>username</th>
            <th>age</th>
            <th>number of todos</th>
        </tr>
    </thead>
    <tbody>
        <% results.map(item => {%>
            <tr>
               <td> <%= item._id.username %> </td>
               <td><%= item._id.age %></td>
               <td><%= item.total %></td>
            </tr>
            
            <% }) %>

    </tbody>
</table>
```   
## Exercises
1. Try to have it only report on the number of completed items!
2. Have it also report the average priority of each user's completed items
       
