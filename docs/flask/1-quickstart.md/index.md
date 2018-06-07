# Flask - Quickstart
## A Minimal Application
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_word():
    return 'Hello, World'
```

Run the application
```bash
export FLASK_APP=source.py
flask run
```

## Debug Mode
```bash
export FLASK_ENV=development
flask run
```
This does the following things:
- it activates the debugger
- it activates the automatic reloader
- it enables the debug mode on the Flask application.

## Routing
Use the route() decorator to bind a function to a URL.
```python
@app.route('/')
def index():
    return 'Index Page'

@app.route('/hello')
def hello():
    return 'Hello, World'
```

## Variable Rules
- add variable sections to a URL by marking sections with `<variable_name>`
- converter to specify the type of the argument like `<converter:variable_name>`

```python
@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % username

@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return 'Post %d' % post_id

@app.route('/path/<path:subpath>')
def show_subpath(subpath):
    # show the subpath after /path/
    return 'Subpath %s' % subpath
```

### Converter types:
- `string`	(default) accepts any text without a slash
- `int`	accepts positive integers
- `float`	accepts positive floating point values
- `path`	like string but also accepts slashes
- `uuid`	accepts UUID strings

## Unique URLs / Redirection Behavior
```python
@app.route('/projects/') # visit projects => projects/
def projects():
    return 'The project page'

@app.route('/about') # visit about/ => 404
def about():
    return 'The about page'
```

## URL Building
To build a URL to a specific function, use the url_for() function. It accepts the name of the function as its first argument and any number of keyword arguments, each corresponding to a variable part of the URL rule. Unknown variable parts are appended to the URL as query parameters.

## HTTP Methods
Use the methods argument of the route() decorator to handle different HTTP methods.
```python
from flask import request

@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()
```
If GET is present, Flask automatically adds support for the HEAD method and handles HEAD requests according to the HTTP RFC. Likewise, OPTIONS is automatically implemented for you.

## Static Files
- create a folder called static in your package or next to your module
- contents will be available at `/static` on the application
- to generate URLs for static files, use the special '`static`' endpoint name
```python
url_for('static', filename='style.css')
```

## Rendering Templates
- Flask configures the Jinja2 template engine automatically.
- use the `render_template()` method to render a template.
```python
from flask import render_template

@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)
```
- Flask will look for templates in the templates folder.
- Have access to the `request`, `session` and `g` objects as well as the `get_flashed_messages()` function inside templates.

## Accessing Request Data
### Context Locals
Certain objects in Flask are global objects, but not of the usual kind. These objects are actually proxies to objects that are local to a specific context

### The Request Object
- import `request` from the `flask` module
```python
from flask import request
```
- current request method is available by using the `method` attribute
- access form data use the `form` attribute
```python
@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        if valid_login(request.form['username'],
                       request.form['password']):
            return log_the_user_in(request.form['username'])
        else:
            error = 'Invalid username/password'
    # the code below is executed if the request method
    # was GET or the credentials were invalid
    return render_template('login.html', error=error)
```
- if the key does not exist in the form attribute, a special `KeyError` is raised and a HTTP 400 Bad Request error page is shown.
- access parameters submitted in the URL `(?key=value)` you can use the `args` attribute:

### File Uploads
- Uploaded files are stored in memory or at a temporary location on the filesystem.
- Those files are accessed by looking at the `files` attribute on the request object.
- Like a standard Python file object, but it also has a save() method that allows you to store that file on the filesystem of the server
```python
from flask import request

@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        f.save('/var/www/uploads/uploaded_file.txt')
    ...
```

### Cookies
- the `cookies` attribute of request objects is a dictionary with all the cookies the client transmits
- access cookies use the `cookies` attribute
- set cookies you can use the `set_cookie` method of `response` objects
```python
# Reading cookies:
from flask import request

@app.route('/')
def index():
    username = request.cookies.get('username')
    # use cookies.get(key) instead of cookies[key] to not get a
    # KeyError if the cookie is missing.

# Storing cookies:
from flask import make_response

@app.route('/')
def index():
    resp = make_response(render_template(...))
    resp.set_cookie('username', 'the username')
    return resp
```

### Redirects and Errors
- redirect a user to another endpoint, use the redirect() function
- abort a request early with an error code, use the abort() function
```python
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()
```
- customize the error page, you can use the `errorhandler()` decorator
```python
from flask import render_template

@app.errorhandler(404)
def page_not_found(error):
    return render_template('page_not_found.html'), 404
```

## About Responses
- The return value from a view function is automatically converted into a response object.
- If the return value is a string it’s converted into a response object with the string as response body, a 200 OK status code and a text/html mimetype.
- Logics:
    - If a response object of the correct type is returned it’s directly returned from the view.
    - If it’s a string, a response object is created with that data and the default parameters.
    - If a tuple is returned the items in the tuple can provide extra information. Such tuples have to be in the form `(response, status, headers)` or `(response, headers)` where at least one item has to be in the tuple. The status value will override the status code and headers can be a list or dictionary of additional header values.
    - If none of that works, Flask will assume the return value is a valid WSGI application and convert that into a response object.
- Get the resulting response object inside the view can use the `make_response()` function.
```python
@app.errorhandler(404)
def not_found(error):
    resp = make_response(render_template('error.html'), 404)
    resp.headers['X-Something'] = 'A value'
    return resp
```

## Sessions
- `session` object allows you to store information specific to a user from one request to the next.
- implemented on top of cookies for you and signs the cookies cryptographically.
```python
from flask import Flask, session, redirect, url_for, escape, request

app = Flask(__name__)

# Set the secret key to some random bytes. Keep this really secret!
app.secret_key = b'_5#y2L"F4Q8z\n\xec]/'

@app.route('/')
def index():
    if 'username' in session:
        return 'Logged in as %s' % escape(session['username'])
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
```
- The `escape()` mentioned here does escaping if not using the template engine  

**Use the following command to quickly generate a value for `Flask.secret_key` (or `SECRET_KEY`):**
```python
import os;
print(os.urandom(16))
=> b'_5#y2L"F4Q8z\n\xec]/'
```

## Logging
As of Flask 0.3 a logger is preconfigured to use.
```python
app.logger.debug('A value for debugging')
app.logger.warning('A warning occurred (%d apples)', 42)
app.logger.error('An error occurred')
```

## Deployments
> pass