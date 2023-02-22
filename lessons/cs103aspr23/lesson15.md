# Writing Web Applications in Python with Flask

We'll be going over the [Quickstart tutorial](https://flask.palletsprojects.com/en/2.2.x/quickstart/#a-minimal-application)
at the [Flask website](https://flask.palletsprojects.com/en/2.2.x/)

## simple example
Here is a minimal example 
``` python
''' hello.py is a simple webserver '''
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"

if __name__=='__main__':
    app.run(debug=True,port=5001)
```
We need to first install the flask package
``` bash
pip3 install flask
```
and run this as usual
``` bash
python3 hello.py
```
and we can then access the webserver by visiting the following link:
[http://localhost:5001](http://localhost:5001)


## the app.route decorator @app.route(PATH)
Flask specifies server behavior using the @app.route(/PATH) decorator
which precedes the function that is to be called when the user visits that PATH.

decorators are "syntactic sugar" for applying a function to a defined function after it is defined, e.g.
the following two code snippets are equivalent 
``` python
def f(arg):
    ...
f = g(f)
f = h(a)(f)
#---------------
@g
@h(a)
def f(arg):
    ...
```
where we assume that we have already defined a function g whose argument is a function and which returns a function.
Likewise h(a) must take a function argument and return a function.

In this case, app.route(PATH) modifies the app server so that it responds to the path http://domain/PATH by calling the function.



