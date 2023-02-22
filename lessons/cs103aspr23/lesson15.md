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


## Sessions
Next we look at a more complex example in which we use cookies to maintain state during a session.

Webservers are designed to be stateless... that is they process each incoming request from a browser
independently of all other requests.  When a user visits a website though they usually only login once
and every other time they visit the site, they don't have to repeat the login information.  This is done
by having the browsers store info from the server (called a cookie) and send it back whenever it visits that
same server. The Flask session handles all of this for us and lets imagine that the server has a dictionary
for each user that visits the site. The dictionary usually resets to an empty dictionary if the browser doesn't
visit the site for some fixed time.

When we impot the session dictioary from flask it allows us to store dictionary values in the cookies
sent between the server and the browser. Here is an example.


``` python

from flask import session,request,redirect,url_for,Flask


app = Flask(__name__)

# Set the secret key to some random bytes. Keep this really secret!
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'

@app.route('/')
def index():
    if 'username' in session:
        return f'Logged in as {session["username"]}'
    return 'You are not logged in'

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        session['username'] = request.form['username']
        return redirect(url_for('index'))
    return '''
        <form method="post">
            <p><input type=text name=username>
            <p><input type=submit value=Login>
        </form>
    '''

@app.route('/logout')
def logout():
    # remove the username from the session if it's there
    session.pop('username', None)
    return redirect(url_for('index'))

if __name__=='__main__':
    app.run(debug=True,port=5001)
    
```

This example also shows how the flask server can get info from a user (via an HTML form) and send it back

## GET and POST requests
Each request from a browser is either a GET request (for example from clicking a link or cut/pasting in a URL into a browser)
or a POST request (for example, from filling out a form and pressing the submit button). The ``` login``` route above
shows the standard way to get info from a user (by sending them a form to fill out) and how to read the data they send (using the
request.form dictionary)

In this example, when the user logs in, we store their username in the session dictionary with the 'username' key
When they visit the '/logout' route, we pop the 'username' key out of the dictionary.

## JINJA
Mixing Python and HTML code makes for confusing programs and is not tenable for large applications.
Flask apps usually use the [JINJA](https://jinja.palletsprojects.com/en/3.1.x/) templating engine to separate markup code (HTML) from Python code.

Below is a more complex model which uses JINJA templating and also stores data in a REDIS repository
This example is in a folder ``` flash_demo`` containing the following files and folders:
``` 
flash_demo.py

static
   hello.txt

templates:
   counter.html
   index.html
   layout.html
   login.html
```
and we run it using the command
``` bash
python3 flash_demo.py
```

This example shows two types of persistent data.
We have already seen how to maintain data for each session, but most webapps need to retain data between sessions as well,
e.g. to store usernames and passwords when someone signs up or logs in.  We'll show how to do this with the [SQLITE](https://www.sqlite.org/index.html) database later,
but for this example we will use the [redis](https://redis.io/) key/value server which we can think of as a dictionary web service.

``` python
''' flash_demo.py shows how to persist data globally and between sessions ```
from flask import Flask, \
     flash, redirect, render_template, url_for, \
     request, session
import redis

app = Flask(__name__)
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'
r = redis.Redis(host='localhost', port=6379, db=0)

@app.route('/')
def index():
    r.set('abc',0)
    session['counter']=0
    if 'username' in session:
        return render_template('index.html',name=session['username'])
    return redirect(url_for('login'))
    

@app.route('/login', methods=['GET', 'POST'])
def login():
    error = None
    if request.method == 'POST':
        if request.form['username'] != 'admin' or \
                request.form['password'] != 'secret':
            error = 'Invalid credentials'
        else:
            flash('You were successfully logged in')
            session['username']=request.form['username']
            return redirect(url_for('index'))
    return render_template('login.html', error=error)

@app.route('/logout')
def logout():
    session.pop('username')
    flash('user has been logged out')
    return redirect(url_for('index'))

@app.route('/counter')
def counter():
    r.set('abc',1+int(r.get('abc')))
    session['counter'] = 1 + session['counter']
    return render_template('counter.html',\
                           global_counter=int(r.get('abc')),\
                            local_counter=session['counter'])



if __name__=='__main__':
    app.run(debug=True,port=5001)

```



