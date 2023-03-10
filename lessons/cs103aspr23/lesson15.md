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
def h(a):
    def f_h(arg):
        ...
    return f_h
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
You can learn more about decorators by searching for Python tutorials like [this one](https://realpython.com/primer-on-python-decorators/)
or [this](https://peps.python.org/pep-0318/)

## Form data and URL data
Here is an example showing how to send data back and forth between browser and server.

``` python
'''
  this demo shows how to pass data to and from a flask server
'''
from flask import Flask,request

app = Flask(__name__)

@app.route("/")
def index():
    return '''
    <h1>Flask Demo</h1>
    <ul>
      <li><a href="/factors_of/120">factors of 120</a></li>
      <li><a href="/factor_demo">factor demo</a></li>
    </ul>
    '''

@app.route('/factors_of/<num>')
def factors_of(num):
    num=int(num)
    factors = [d for d in range(1,num+1) if num%d==0]
    return factors

@app.route('/factor_demo', methods=['GET', 'POST'])
def factor_demo():
    if request.method == 'GET':
        return '''
        <form method="POST" action="/factor_demo">
          Enter a number to factor: <input type="text" name="num"><br>
          <input type="submit">
        </form>
        '''
    elif request.method == 'POST':
        num=int(request.form['num'])
        factors = [d for d in range(1,num+1) if num%d==0]
        return factors
    else:
        return 'unknown HTTP method: '+str(request.method)




if __name__=='__main__':
    app.run(debug=True,port=5001)

```

## Creating a GPT-based web app
Next we show how to create a GPT based web app in flask. This is very basic but you can make it fancier if you want.
First we define a GPT class to make it easier to call GPT from the web app.

``` python
'''
Demo code for interacting with GPT-3 in Python.

To run this you need to 
* first visit openai.com and get an APIkey, 
* which you export into the environment as shown in the shell code below.
* next create a folder and put this file in the folder as gpt.py
* finally run the following commands in that folder

On Mac
% pip3 install openai
% export APIKEY="......."  # in bash
% python3 gpt.py

On Windows:
% pip install openai
% $env:APIKEY="....." # in powershell
% python gpt.py
'''
import openai


class GPT():
    ''' make queries to gpt from a given API '''
    def __init__(self,apikey):
        ''' store the apikey in an instance variable '''
        self.apikey=apikey
        # Set up the OpenAI API client
        openai.api_key = apikey #os.environ.get('APIKEY')

        # Set up the model and prompt
        self.model_engine = "text-davinci-003"

    def getResponse(self,prompt):
        ''' Generate a GPT response '''
        completion = openai.Completion.create(
            engine=self.model_engine,
            prompt=prompt,
            max_tokens=1024,
            n=1,
            stop=None,
            temperature=0.8,
        )

        response = completion.choices[0].text
        return response

if __name__=='__main__':
    '''
    '''
    import os
    g = GPT(os.environ.get("APIKEY"))
    print(g.getResponse("what does openai's GPT stand for?"))
```

Once we have this, we can create the webapp which generates a form to get the user's query
and then calls GPT and sends the result back to the browser.

``` python
'''
gptwebapp shows how to create a web app which ask the user for a prompt
and then sends it to openai's GPT API to get a response. You can use this
as your own GPT interface and not have to go through openai's web pages.

We assume that the APIKEY has been put into the shell environment.
Run this server as follows:

On Mac
% pip3 install openai
% pip3 install flask
% export APIKEY="......."  # in bash
% python3 gptwebapp.py

On Windows:
% pip install openai
% pip install flask
% $env:APIKEY="....." # in powershell
% python gptwebapp.py
'''
from flask import request,redirect,url_for,Flask
from gpt import GPT
import os

app = Flask(__name__)
gptAPI = GPT(os.environ.get('APIKEY'))

# Set the secret key to some random bytes. Keep this really secret!
app.secret_key = b'_5#y2L"F4Q789789uioujkkljkl...8z\n\xec]/'

@app.route('/')
def index():
    ''' display a link to the general query page '''
    print('processing / route')
    return f'''
        <h1>GPT Demo</h1>
        <a href="{url_for('gptdemo')}">Ask questions to GPT</a>
    '''


@app.route('/gptdemo', methods=['GET', 'POST'])
def gptdemo():
    ''' handle a get request by sending a form 
        and a post request by returning the GPT response
    '''
    if request.method == 'POST':
        prompt = request.form['prompt']
        answer = gptAPI.getResponse(prompt)
        return f'''
        <h1>GPT Demo</h1>
        <pre style="bgcolor:yellow">{prompt}</pre>
        <hr>
        Here is the answer in text mode:
        <div style="border:thin solid black">{answer}</div>
        Here is the answer in "pre" mode:
        <pre style="border:thin solid black">{answer}</pre>
        <a href={url_for('gptdemo')}> make another query</a>
        '''
    else:
        return '''
        <h1>GPT Demo App</h1>
        Enter your query below
        <form method="post">
            <textarea name="prompt"></textarea>
            <p><input type=submit value="get response">
        </form>
        '''

if __name__=='__main__':
    # run the code on port 5001, MacOS uses port 5000 for its own service :(
    app.run(debug=True,port=5001)
```



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

Below is a more complex model which uses JINJA templating and also stores data in a REDIS repository.
Redis is a simple NOSQL database which stands for Remote Dictionary Server.  It is basically a serice that lets you store and retrieve key/value pairs.

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

## Python code for a more sophisticated app   
    
This example shows two types of persistent data.

We have already seen how to maintain data for each session, but most webapps need to retain data between sessions as well,
e.g. to store usernames and passwords when someone signs up or logs in.  We'll show how to do this with the [SQLITE](https://www.sqlite.org/index.html) database later,
but for this example we will use the [redis](https://redis.io/) key/value server which we can think of as a dictionary web service.
Once you've installed the redis code, you can start it by the command
``` bash
redis-server
```
and this starts the server on the standard port, 6379, for redis.
Redis stands for "Remote Dictionary Service". You set key/value pairs using
``` r.set(key,value)``` and you get them with ```r.get(key)```
You need to import the redis package and create the redis interface with
a call to ```redis.Redis(host=...., port=..., db=...) ``` as shown below.

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


## Layout and inheritance of templates
Below is the file layout.html
which defines the shape of all webpage generated by this server.
It also shows how JINJA mixes HTML (where the markup is given by \<tags\>)
and python (which is enclosed in {%....%} delimiters for control structures
and {{....}} delimiters for expressions
This layout will create a webpage with some messages at the top
and some content (using the block body tag) at the bottom
    
``` html
<!doctype html>
<title>My Application</title>
{% with messages = get_flashed_messages() %}
  {% if messages %}
    <ul class=flashes>
    {% for message in messages %}
      <li>{{ message }}</li>
    {% endfor %}
    </ul>
  {% endif %}
{% endwith %}
{% block body %}{% endblock %}

```
    
### index.html
Here is the index.html file which will be rendered for the "/" route.
The "extends" statement indicates that this JINJA file extends the layout.html file above
by changing the contents of the "body block"
``` html
{% extends "layout.html" %}
{% block body %}
  <h1>Overview</h1>
  <p>Welcome {{name}}</p>
  <p>Visit the <a href="{{url_for('counter')}}">counter page</a></p>
  <p>Visit a static page:<a href="/static/hello.txt">hello.txt</a></p>
  <p>Do you want to <a href="{{ url_for('logout') }}">log out?</a>
{% endblock %}
```
That is, it generates the html page by plugging in the "body" block into the block body section of layout.html

      
### login.html
Here is a login page which asks for a username and password (and the only pair that works
is the very insecure admin/secret  pair).
``` html
{% extends "layout.html" %}
{% block body %}
  <h1>Login</h1>
  {% if error %}
    <p class=error><strong>Error:</strong> {{ error }}
  {% endif %}
  <form method=post>
    <dl>
      <dt>Username:
      <dd><input type=text name=username value="{{
          request.form.username }}">
      <dt>Password:
      <dd><input type=password name=password>
    </dl>
    <p><input type=submit value=Login>
  </form>
{% endblock %}
```
    
### counter.html
This page shows two counters.  
One (local_counter) that is only affected by the current session (even if there are thousands of simulataneous users)
and another that is affected by all users (global_counter)
``` html
{% extends "layout.html" %}
{% block body %}
  <h1>Testing</h1>
  <p>the counter shows the number of times you visited this page.
    It is reset when you go to the index page. The global is reset
    if anyone goes to the index page and is incremented if anyone visits
    that page. The local only resets when the user visits index or 
    refreshes the counter page.
  </p>
  <p>local_counter = {{local_counter}}</p>
  <p>global_counter = {{global_counter}}</p>
  <p><a href="{{url_for('counter')}}">refresh this page</a></p>
  <p>Visit a static page:<a href="/static/hello.txt">hello.txt</a></p>
  <p>Return to the <a href="{{url_for('index')}}">index page</a></p>
{% endblock %}


```
    


