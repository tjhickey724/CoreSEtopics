# Mongoose queries
Today we explore the variety of [Mongoose](https://mongoosejs.com/) queries one can use to interact with a database.
The four main kinds of interaction are referred to as the CRUD operations
* Create =-  Model.insert(JSON_object)
* Read - Model.find(FILTER,PROJECION)  or Model.findOne(....)
* Update - [Model.findOneAndUpdate(FILTER,...)](https://mongoosejs.com/docs/tutorials/findoneandupdate.html)
* Delete - Model.deleteOne(FILTER) or Model.deleteMany

[Here](https://mongoosejs.com/docs/queries.html) is a list of all of the Mongoose queries

But it is helpful to also be able to use the raw [Mongodb queries](https://www.mongodb.com/docs/manual/crud/)

To explore this we will modify the todo app to allow us to mark a task as completed
and also to add priorities to tasks and to change those priorities.
