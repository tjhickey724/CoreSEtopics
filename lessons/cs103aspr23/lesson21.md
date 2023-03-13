# Using Fixtures in automated testing

Today we look into using pytest to test functionality of programs operating on a database.

We begin by looking over the [documentation for pytest fixtures](https://docs.pytest.org/en/6.2.x/fixture.html)

Then we show how to test the todolist ORM using pytest with fixtures.

We start with a slight modification of the TodoList class in which we pass the path
to the database into the constructor.

Here is the todo3.py file that uses this ORM
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

from todolist3 import TodoList
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
    
    todolist = TodoList('todo.db')  # ******* Here is where we create the ORM
    
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
and here is the todolist3.py file that defines the TodoList ORM class 

``` python
'''
TodoList is an Object Relational Mapping to the todo.db database

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

def to_dict(t):
    ''' t is a tuple (rowid,title, desc,completed)'''
    todo = {'rowid':t[0], 'title':t[1], 'desc':t[2], 'completed':t[3]}
    return todo

def tuples_to_dicts(ts):
    return [to_dict(t) for t in ts]

class TodoList():
    def __init__(self,db_path):
        self.db_path=db_path
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
        #con= sqlite3.connect(os.getenv('HOME')+'/todo.db')
        con = sqlite3.connect(self.db_path)
        cur = con.cursor() 
        cur.execute(query,tuple)
        tuples = cur.fetchall()
        con.commit()
        con.close()
        return tuples_to_dicts(tuples)
      
```


and here is the code to test it
``` python
'''
simple demo of fixtures for Python program with pytest
use to test the TodoList class 
'''

import pytest
import sqlite3
from todolist3 import TodoList, to_dict, tuples_to_dicts

@pytest.fixture
def tuples():
    " create some tuples to put in the database "
    return [("test1", "run test0", 0), 
            ("test2", "run test1", 1)
           ]

@pytest.fixture
def returned_tuples(tuples):
    " add a rowid to the beginning of each tuple "
    return [(i+1,)+tuples[i] for i in range(len(tuples))]

@pytest.fixture
def returned_dicts(tuples):
    " add a rowid to the beginning of each tuple "
    return tuples_to_dicts([(i+1,)+tuples[i] for i in range(len(tuples))])

@pytest.fixture
def todo_path(tmp_path):
    yield tmp_path / 'todo.db'

@pytest.fixture(autouse=True)
def todolist(todo_path,tuples):
    "create and initialize the todo.db database in /tmp "
    con= sqlite3.connect(todo_path)
    cur = con.cursor()
    cur.execute('''CREATE TABLE IF NOT EXISTS todo
                    (title text, desc text, completed int)''')
    for i in range(len(tuples)):
        cur.execute('''insert into todo values(?,?,?)''',tuples[i])
    # create the todolist database
    con.commit()
    td = TodoList(todo_path)
    yield td
    cur.execute('''drop table todo''')
    con.commit()


def test_selectAll(todolist,returned_dicts):
    td = todolist
    results = td.selectAll()
    expected = returned_dicts
    assert results == expected
  
def test_selectActive(todolist,returned_dicts):
    td = todolist
    results = td.selectActive()
    expected = [d for d in returned_dicts if d['completed']==0]
    assert results == expected

def test_selectCompleted(todolist,returned_dicts):
    td = todolist
    results = td.selectCompleted()
    expected = [d for d in returned_dicts if d['completed']==1]
    assert results == expected

def test_add(todolist,returned_dicts):
    td = todolist
    tuple = (len(returned_dicts)+1,'test','write add test',1)
    todolist.add(to_dict(tuple))
    results = td.selectAll()
    assert results[-1] == to_dict(tuple)

def test_delete(todolist,returned_dicts):
    td = todolist
    td.delete(1)
    results = td.selectAll()
    expected = returned_dicts
    assert results == expected[1:]

def test_set_complete(todolist,returned_dicts):
    td = todolist
    td.setComplete(1) 
    results = td.selectActive()
    assert results==[]

```

and here is how we run pytest on this test
``` python
(base) tim@macbook-pro-3 lesson21 % pytest -vv
================================================== test session starts ===================================================
platform darwin -- Python 3.10.7, pytest-7.2.1, pluggy-1.0.0 -- /Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10
cachedir: .pytest_cache
rootdir: /Users/tim/Desktop/cs103a/lesson21
collected 7 items                                                                                                        

test_tmppath.py::test_create_file PASSED                                                                           [ 14%]
test_todolist.py::test_selectAll PASSED                                                                            [ 28%]
test_todolist.py::test_selectActive PASSED                                                                         [ 42%]
test_todolist.py::test_selectCompleted PASSED                                                                      [ 57%]
test_todolist.py::test_add PASSED                                                                                  [ 71%]
test_todolist.py::test_delete PASSED                                                                               [ 85%]
test_todolist.py::test_set_complete PASSED                                                                         [100%]

=================================================== 7 passed in 0.05s ====================================================
(base) tim@macbook-pro-3 lesson21 % 
```

