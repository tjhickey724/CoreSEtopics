# Persistence 
Today we look at using SQL in an app to provide persistent memory.
Our example will be a todo list all in Python as a shell command

Here is our initial code:
``` python
#! /opt/miniconda3/bin/python3
'''
todo.py is an app that maintains a todo list
It is meant to be used as a shell command 
% todo show  .... shows all of the uncompleted todo items
% todo showall ... shows al of the todo items
% todo add "title" "description of an item"  ... adds the itme
% todo complete ITEM_NUM ... marks that item as completed
% todo delete ITEM_NUM ... removes the item from the database

This app will store the data in a SQLite database ~/todo.db

Recall that sys.argv is a list of strings capturing the
command line invocation of this program
sys.argv[0] is the name of the script invoked from the shell
sys.argv[1:] is the rest of the arguments (after arg expansion!)

'''

import sqlite3
import sys
import os

# here are some helper functions ...

def print_usage():
    ''' print an explanation of how to use this command '''
    print('''usage:
            todo show
            todo showall
            todo showcomplete
            todo add name description
            todo complete item_id
            todo delete item_id
            '''
            )

def print_todos(todos):
    ''' print the todo items '''
    for item in todos:
            print("item #%-3s: %-10s %30s %2d"%item)

def process_args(arglist):
    if arglist==[]:
        print_usage()
    elif arglist[0]=="show":
        cur.execute("SELECT rowid,title,desc,completed from todo where completed=0")
        print_todos(cur.fetchall())
    elif arglist[0]=="showall":
        cur.execute("SELECT rowid,* from todo")
        print_todos(cur.fetchall())
    elif arglist[0]=="showcomplete":
        cur.execute("SELECT rowid,* from todo where completed=1")
        print_todos(cur.fetchall())
    elif arglist[0]=='add':
        if len(arglist)!=3:
            print_usage()
        else:
            name=arglist[1]
            desc = arglist[2]
            cur.execute("INSERT INTO todo VALUES(?,?,?)",(name,desc,0))
    elif arglist[0]=='complete':
        if len(arglist)!= 2:
            print_usage()
        else:
            cur.execute("UPDATE todo SET completed=1 where rowid=(?)",(arglist[1],))
    elif arglist[0]=='delete':
        if len(arglist)!= 2:
            print_usage()
        else:
            cur.execute("DELETE FROM todo where rowid=(?)",(arglist[1],))
    else:
        print(arglist,"is not implemented")
        print_usage()

# here is where we run the script
# first we get a connection to the database
con= sqlite3.connect(os.getenv('HOME')+'/todo.db')
cur = con.cursor()

# then we create the todo table if it doesn't exist
# and for completed we use 0 for False and 1 for True, as sqlite has not booleans
cur.execute('''CREATE TABLE IF NOT EXISTS todo
                    (title text, desc text, completed int)''')

if len(sys.argv)==1:
    # they didn't pass any arguments, so prompt for them:
    print_usage()
    args = input("command> ").split(" ")
else:
    args = sys.argv[1:]
# now we can get process the arguments
process_args(args)

# and finally we commit our changes and close the connection
con.commit()
con.close()
```

## Object Relational Models
We can refactor the todo app above by creating an Object Relational Model
which is a Python class that implements a list of todo items as dictionaries,
but stores the data as rows in a SQLite database.  This completely hides the
SQL code from the application that uses it (todo2). It allows us to test the
SQL code separately from the app.

Here is the code for the TodoList ORM:

``` python
'''
todolist.py is an Object Relational Mapping to the todolist database

The ORM will work map SQL rows with the schema
    (rowid,title,desc,completed)
to Python Dictionaries as follows:

(5,'commute','drive to work',false) <-->
{rowid:5,title:'commute',desc:'drive to work',completed:false)

In place of SQL queries, we will have method calls.

This app will store the data in a SQLite database ~/todo.db

'''
import sqlite3
import os

def toDict(t):
    ''' t is a tuple (rowid,title, desc,completed)'''
    print('t='+str(t))
    todo = {'rowid':t[0], 'title':t[1], 'desc':t[2], 'completed':t[3]}
    return todo

class TodoList():
    def __init__(self):
        self.runQuery('''CREATE TABLE IF NOT EXISTS todo
                    (title text, desc text, completed int)''',())
    
    def selectActive(self):
        ''' return all of the uncompleted tasks as a list of dicts.'''
        return self.runQuery("SELECT rowid,* from todo where completed=0",())

    def selectAll(self):
        ''' return all of the tasks as a list of dicts.'''
        return self.runQuery("SELECT rowid,* from todo",())

    def selectCompleted(self):
        ''' return all of the completed tasks as a list of dicts.'''
        return self.runQuery("SELECT rowid,* from todo where completed=1",())

    def add(self,item):
        ''' create a todo item and add it to the todo table '''
        return self.runQuery("INSERT INTO todo VALUES(?,?,?)",(item['title'],item['desc'],item['completed']))

    def delete(self,rowid):
        ''' delete a todo item '''
        return self.runQuery("DELETE FROM todo WHERE rowid=(?)",(rowid,))

    def setComplete(self,rowid):
        ''' mark a todo item as completed '''
        return self.runQuery("UPDATE todo SET completed=1 WHERE rowid=(?)",(rowid,))

    def runQuery(self,query,tuple):
        ''' return all of the uncompleted tasks as a list of dicts.'''
        con= sqlite3.connect(os.getenv('HOME')+'/todo.db')
        cur = con.cursor() 
        cur.execute(query,tuple)
        tuples = cur.fetchall()
        con.commit()
        con.close()
        return [toDict(t) for t in tuples]

```

and here is the revised todo app that uses this ORM.
You can see how much clearer it is that the original code.

``` python
#! /opt/miniconda3/bin/python3
'''
todo2 is an app that maintains a todo list
just as with the todo code in this folder.

but it also uses an Object Relational Mapping (ORM)
to abstract out the database operations from the
UI/UX code.

The ORM, TodoList, will map SQL rows with the schema
    (rowid,title,desc,completed)
to Python Dictionaries as follows:

(5,'commute','drive to work',false) <-->

{rowid:5,
 title:'commute',
 desc:'drive to work',
 completed:false)
 }

In place of SQL queries, we will have method calls.

This app will store the data in a SQLite database ~/todo.db

Recall that sys.argv is a list of strings capturing the
command line invocation of this program
sys.argv[0] is the name of the script invoked from the shell
sys.argv[1:] is the rest of the arguments (after arg expansion!)

Note the actual implementation of the ORM is hidden and so it 
could be replaced with PostgreSQL or Pandas or straight python lists

'''

from todolist import TodoList
import sys


# here are some helper functions ...

def print_usage():
    ''' print an explanation of how to use this command '''
    print('''usage:
            todo show
            todo showall
            todo showcomplete
            todo add name description
            todo complete item_id
            todo delete item_id
            '''
            )

def print_todos(todos):
    ''' print the todo items '''
    if len(todos)==0:
        print('no tasks to print')
        return
    print('\n')
    print("%-10s %-10s %-30s %-10s"%('item #','title','desc','completed'))
    print('-'*40)
    for item in todos:
        values = tuple(item.values()) #(rowid,title,desc,completed)
        print("%-10s %-10s %-30s %2d"%values)

def process_args(arglist):
    ''' examine args and make appropriate calls to TodoList'''
    todolist = TodoList()
    if arglist==[]:
        print_usage()
    elif arglist[0]=="show":
        print_todos(todolist.selectActive())
    elif arglist[0]=="showall":
        print_todos(todos = todolist.selectAll())
    elif arglist[0]=="showcomplete":
        print_todos(todolist.selectCompleted())
    elif arglist[0]=='add':
        if len(arglist)!=3:
            print_usage()
        else:
            todo = {'title':arglist[1],'desc':arglist[2],'completed':0}
            todolist.add(todo)
    elif arglist[0]=='complete':
        if len(arglist)!= 2:
            print_usage()
        else:
            todolist.setComplete(arglist[1])
    elif arglist[0]=='delete':
        if len(arglist)!= 2:
            print_usage()
        else:
            todolist.delete(arglist[1])
    else:
        print(arglist,"is not implemented")
        print_usage()


def toplevel():
    ''' read the command args and process them'''
    if len(sys.argv)==1:
        # they didn't pass any arguments, 
        # so prompt for them in a loop
        print_usage()
        args = []
        while args!=['']:
            args = input("command> ").split(' ')
            if args[0]=='add':
                # join everyting after the name as a string
                args = ['add',args[1]," ".join(args[2:])]
            process_args(args)
            print('-'*40+'\n'*3)
    else:
        # read the args and process them
        args = sys.argv[1:]
        process_args(args)
        print('-'*40+'\n'*3)

    

toplevel()


```
