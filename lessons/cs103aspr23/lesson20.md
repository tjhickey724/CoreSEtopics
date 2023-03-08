# SQL tutorial
We spend a few minutes going through parts of the [SQLtutorial](https://www.sqltutorial.org/)

We begin by cd'ing into the sqltutorial folder which contains the files create_tables.sql and load_tables.sql
then we start up the session with the following commands:

``` bash
sqlite3
.open demo.db
-- creates or loads the demo.db database file

.read create_tables.sql
-- processes the sql commands in create_tables.sql

.read load_tables.sql
-- processes the sql commands in load_tables.sql

.tables
-- shows all of the tables in the database

select count(*) from employees; -- returns the number of employees

.schema employees
-- shows the column names and types for the employees table
```

Then we look over the documentation for 
* JOIN 

