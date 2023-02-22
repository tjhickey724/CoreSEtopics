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
