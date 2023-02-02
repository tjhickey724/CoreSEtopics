# Object-Oriented Programming in Python

In this lesson we give an intro to Python for Java programmers
and also show how to create classes and objects in Python

## Motivation
Object Oriented Languages are the most common programming languages and software developers need to be able to switch from one OOP language to another quickly. Today we look at how to create classes and object in Python and compare it to Java.
We will also use Python tutor to explain classes and objects in Python ..


## Learning Objectives -- by the end of the lesson you will be able to
create simple classes in Python and use them to create objects
trace through the execution of a program which uses classes and objects
use the IDLE debugger to set breakpoints and step through a program



## Activity -- Our first Python Class
``` python
'''
BankAcccount class is a demo for Object Oriented Programming in Python
'''
class BankAccount():
    ''' represents a bank account you can deposit and withdraw from'''
    def __init__(self,name,starting_balance):
        self.name = name
        self.balance = starting_balance
        
    def __str__(self):
        return "["+self.name+","+str(self.balance)+"]"
    
    def get_balance(self):
        return self.balance
    
    def get_name(self):
        return self.name
    
    def deposit(self,amount):
        self.balance += amount
        
    def withdraw(self,amount):
        self.balance -= amount
        
    def transfer(self, account, amount):
        self.balance -= amount
        account.balance += amount
```

### Points to discuss:

* constructor is __init__(self, ....)
* every instance method needs to have first parameter be "self"
* objects are created with a call to the constructor
``` python 
a = BankAccount('tim',1000)
```
* methods are called using the familiar dot notation
``` python
a.deposit(234)
```



## Activity -- Tracing with PythonTutor
We use the PythonTutor to walk through the creation and use of a Python class, BankAccount from the reading.
https://pythontutor.com/visualize.html#mode=display

Wait until I'm done before playing with this as we don't want to overload the python tutor server.



## Activity -- Debugging with IDLE
We show how to step through a program using the IDLE debugger
We'll use the polygon.py example to show debugging.
Key ideas:

turning debug mode on/off
stepping through a program, stepping in, over, out
setting breakpoints in a program

## Activity -- Python Coding Style
We install the pylint command and use it to get our code to meet the PEP8 python coding style guidelines
https://pylint.org/
pip3 install pylint     (on Mac)
pip install pylint    (on Windows in powershell)

Here is the coding style guide accepted by the community
[pep-0008](https://www.python.org/dev/peps/pep-0008/)

We also look over some other documents about coding style in Python
[zen-of-python](https://docs.python-guide.org/writing/style/#zen-of-python)



