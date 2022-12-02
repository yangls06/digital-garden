---
title: "Flask上手"
tags:
- setup
weight: -5
---

# Flask上手

## Reference

[Flask Installation](https://flask.palletsprojects.com/en/2.2.x/installation/)

[Flask Tutorial](https://flask.palletsprojects.com/en/2.2.x/tutorial/)

Follow the tutorial to build a blog web app like:



![image-20221201145000837](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221201145000837.png)



## Step by Step

### Step 0.Backgroud

build a basic blog application called Flaskr using Flask.



### Step 1. Project Layout

#### Create a project directory

```shell
mkdir flask-tutorial
cd flask-tutorial/
```



#### Install Flask

[Flask Installation Doc ](https://flask.palletsprojects.com/en/2.2.x/installation/)

Create an python environment using [venv](https://docs.python.org/3/library/venv.html#module-venv):

```shell
$ python3 -m venv venv

$ tree . -L 3
.
└── venv
    ├── bin
    │   ├── Activate.ps1
    │   ├── activate
    │   ├── activate.csh
    │   ├── activate.fish
    │   ├── pip
    │   ├── pip3
    │   ├── pip3.9
    │   ├── python -> python3
    │   ├── python3 -> /Users/yangls06/opt/miniconda3/bin/python3
    │   └── python3.9 -> python3
    ├── include
    ├── lib
    │   └── python3.9
    └── pyvenv.cfg

5 directories, 11 files

$ tree venv/lib/python3.9/ -L 3
venv/lib/python3.9/
└── site-packages
    ├── _distutils_hack
    │   ├── __init__.py
    │   ├── __pycache__
    │   └── override.py
    ├── distutils-precedence.pth
    ├── pip
    │   ├── __init__.py
    │   ├── __main__.py
        ...
    ├── pkg_resources
    ...
```



Activate the environment

```shell
$ . venv/bin/activate
```

Then the environment has been changed.

![image-20221201153333547](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221201153333547.png)

Install Flask

Within the activated environment, install Flask using `pip`:

```shell
$ pip install Flask

Looking in indexes: https://pypi.douban.com/simple
Collecting Flask
  Downloading https://pypi.doubanio.com/packages/0f/43/15f4f9ab225b0b25352412e8daa3d0e3d135fcf5e127070c74c3632c8b4c/Flask-2.2.2-py3-none-any.whl (101 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 101.5/101.5 KB 1.8 MB/s eta 0:00:00
...
Collecting MarkupSafe>=2.0
  Downloading https://pypi.doubanio.com/packages/06/7f/d5e46d7464360b6ac39c5b0b604770dba937e3d7cab485d2f3298454717b/MarkupSafe-2.1.1-cp39-cp39-macosx_10_9_universal2.whl (17 kB)
Installing collected packages: zipp, MarkupSafe, itsdangerous, click, Werkzeug, Jinja2, importlib-metadata, Flask
Successfully installed Flask-2.2.2 Jinja2-3.1.2 MarkupSafe-2.1.1 Werkzeug-2.2.2 click-8.1.3 importlib-metadata-5.1.0 itsdangerous-2.1.2 zipp-3.11.0
```



Git init

```shell
$ git init
```



with `.gitignore`

```
venv/

*.pyc
__pycache__/

instance/

.pytest_cache/
.coverage
htmlcov/

dist/
build/
*.egg-info/
```



Add folders

```shell
$ tree -a -L 1
.
├── .git
├── .gitignore
├── flaskr
├── tests
└── venv

4 directories, 1 file
```



### Step 3. Application Setup

#### The Application Factory: \__init__.py

* create `Flask` instance 
* make `flaskr` directory a package

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

"""
@Time    :   2022/12/01 15:58:24
@Author  :   Linsan Yang 
@Desc    :   init flaskr
"""

import os
from flask import Flask

def create_app(test_config=None):
    # create and configure the app
    app = Flask(__name__, instance_relative_config=True)
    app.config.from_mapping(
        SECRET_KEY = 'dev',
        DATABASE=os.path.join(app.instance_path, 'flaskr.sqlite'),
    )

    if test_config is None:
        # load the instance config, if it exists, when not testing
        app.config.from_pyfile('config.py', silent=True)
    else:
        # load the test config if passed in
        app.config.from_mapping(test_config)
    
    # ensure the instance folder exists
    try:
        os.makedirs(app.instance_path)
    except OSError as e:
        pass

    # a simple page that says hello
    @app.route('/hello')
    def hello():
        return 'Hello, World!'

    return app

```



**instance folder** 

There will be a `instance/`directory, located outside the `flaskr` package and can hold local data that shouldn’t be committed to version control, such as configuration secrets and the database file.



**test_config**

Using test_config for testing.



**@app.route()**

create a simple route of `/hello`



#### Run The Application

In the `flask-tutorial` dir not `flaskr` package:

```shell
$ flask --app flaskr --debug run
 * Serving Flask app 'flaskr'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: 134-914-837
```

Then open 127.0.0.1:5000/hello in browser, got

![image-20221201162507048](https://happy3-data.oss-cn-hangzhou.aliyuncs.com/content-images/image-20221201162507048.png)

### Step 4. Define and Access the Database

The app will use `Sqlite` database to store users and posts. Python has a built-in module `sqlite3` module.

#### Connect to Sqlite

flaskr/db.py

```python
import sqlite3

import click
from flask import current_app, g

def get_db():
    if 'db' not in g:
        g.db = sqlite3.connect(
            current_app.config['DATABASE'],
            detect_types=sqlite3.PARSE_DECLTYPES
        )
        g.db.row_factory = sqlite3.Row
    return g.db

def close_db(e=None):
    db = g.pop(db, None)

    if db is not None:
        db.close()
```



`g` is a spectial object for each request to share data among different functions. `current_app` is similar.



#### Create Tables: using sql

Define `user` and `post` table in `flaskr/schema.sql`:

```sql
DROP TABLE IF EXISTS user;
DROP TABLE IF EXISTS post;

CREATE TABLE user (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  username TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL
);

CREATE TABLE post (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  author_id INTEGER NOT NULL,
  created TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
  title TEXT NOT NULL,
  body TEXT NOT NULL,
  FOREIGN KEY (author_id) REFERENCES user (id)
);
```



Add functions to run the SQLs to the `db.py`

```python
def init_db():
    db = get_db()
    
    with current_app.open_resource('schema.sql') as f:
        db.executescript(f.read().decode('utf8'))

@click.command('init-db')
def init_db_command():
    '''Clear the existing data and create new tables.'''
    init_db()
    click.echo('Initialized the database.')
```



#### Register with the Applicaiton

The close_db and init_db_command functions need to be registered with the app instance for use.

In `db.py` add a new `init_app` function:

```python
def init_app(app):
    # tells Flask to call that function when cleaning up after returning the response
    app.teardown_appcontext(close_db)
    # adds a new command that can be called with the flask command
    app.cli.add_command(init_db_command)
```



Then import and call this function from the factory in `__init__.py`.

```python
def create_app(test_config=None):
    # create and configure the app
    app = ...
    # existing code omitted
    
    # add db functions
    from . import db
    db.init_app(app)

    return app

```



#### Initialize the Database

Now use `init-db`command like this:

```shell
$ flask --app flaskr init-db
Initialized the database.

$ tree instance/
instance/
└── flaskr.sqlite

0 directories, 1 file
```



The command generates a sqlite db file `flaskr.sqlite` in `instance/` dir. 



### Step 5. Blueprints and Views

Referances:

[Blueprints and Views](https://flask.palletsprojects.com/en/2.2.x/tutorial/views/)

[Use a Flask Blueprint to Architect Your Applications](https://realpython.com/flask-blueprint/#:~:text=Flask%20is%20a%20very%20popular,its%20functionality%20into%20reusable%20components)



Concept: view

A view is Flask's respond to the outgoing request. Flask uses patterns to match the incoming request URL to the view that should handle it.



Concept: blueprint

A blueprint is a way to organize a group of related views and other code. Rather than registering views and other code directly with an application, they are registered with a blueprint. Then the blueprint is registered with the application when it is available in the factory function.



#### Create a Blueprint

Flaskr will have two blueprints:

* auth functions
* blog posts functions



`Flaskr/auth.py`

```python
import functools

from flask import (
    Blueprint, flash, g, redirect, render_template, request, session, url_for
)
from werkzeug.security import check_password_hash, generate_password_hash
from flaskr.db import get_db

bp = Blueprint('auth', __name__, url_prefix='/auth')

```

A new `Blueprint` is created:

* with `name`: 'auth'
* with `import_name`: '\__name__', helping the blueprint to know where it’s defined
* with `url_prefix`: will be prepended (added at head)to all the URLs associated with this blueprint.

Then register the blueprint to the app from the factory in the `__init__.py`

```python
def create_app(test_config=None):
    # create and configure the app
    app = ...
    # existing code omitted
    

    # add auth blueprint
    from . import auth
    app.register_blueprint(auth.bp)

    return app
```



> Referances:
>
> [Python functools](https://docs.python.org/3/library/functools.html)



#### Register view

When the user visits the `/auth/register` URL, the `register` view will return [HTML](https://developer.mozilla.org/docs/Web/HTML) with a form for them to fill out. When they submit the form, it will validate their input and either show the form again with an error message or create the new user and go to the login page.



The view code is as following in `flaskr/auth.py`

```python
@bp.route('/register', methods=('GET', 'POST'))
def register():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        db = get_db()
        error = None

        if not username:
            error = 'Username is required.'
        elif not password:
            error = 'Password is required.'

        if error is None:
            try:
                db.execute(
                    'INSERT INTO user (username, password) VALUES (?, ?)',
                    (username, generate_password_hash(password))
                )
                db.commit()
            except db.IntegrityError:
                error = f"User {username} is already registered."
            else:
                return redirect(url_for('auth.login'))
        
        flash(error)
    
    return render_template('auth/register.html')
```



The `register` view works as following:

* @bp.route associates the URL `/register` with the `register` view.
* If the user submited the register form, `request.method == 'POST'`, then validate the input `username` and `password` and store them into the database.
* If storing the user info succeeds, then redirect to the `auth.login` page.
* If the user is initially landing on the `register` page, or there was a validation error, the `register.html` will be shown.



#### Login view

This view follows the same pattern as `register` view.

```python
 @bp.route('/login', methods=('GET', 'POST'))
def login():
    if request.method == 'POST':
        username = request.form['username']
        password = request.form['password']
        db = get_db()
        error = None

        user = db.execute(
            'SELECT * FROM user WHERE username = ?', (username,)
        ).fetchone()

        if user is None:
            error = 'Incorrect username.'
        elif not check_password_hash(user['password'], password):
            error = 'Incorrect password.'
        
        if error is None:
            session.clear()
            session['user_id'] = user['id']
            return redirect(url_for('index'))
        
        flash(error)
    
    return render_template('auth/login.html')

```

Tips:

* The user info is queried and stored in `user` variable using `fetch_one` function.
* Validate the `username` and `password` (by `check_password_hash`) inputs.
* The `session` (a dict storing data across requests) refreshes if login succeeds. (The data is stored in a *cookie* that is sent to the browser, and the browser then sends it back with subsequent requests.)



We can get user info at the beginning of each request via `session`:

```python
@bp.before_app_request
def load_logged_in_user():
    user_id = session.get('user_id')

    if user_id is None:
        g.user = None
    else:
        g.user = get_db().execute(
            'SELECT * FROM user WHERE id = ?', (user_id,)
        ).fetchone()
```

Tips:

* [`bp.before_app_request()`](https://flask.palletsprojects.com/en/2.2.x/api/#flask.Blueprint.before_app_request) registers a function that runs before the view function, no matter what URL is requested. 
* `load_logged_in_user`  gets that user info from the database via `session` and stores it on [`g.user`](https://flask.palletsprojects.com/en/2.2.x/api/#flask.g), which lasts for the length of the request. 



#### Logout view

The Logout view removes the user id from the [`session`](https://flask.palletsprojects.com/en/2.2.x/api/#flask.session). 

```python
@bp.route('/logout')
def logout():
    session.clear()
    return redirect(url_for('index'))
```



#### Require Auth in Other views

Creating, editing and deleting blog posts requires the user to be logged in. Use a **decorator** to achieve this.

```python
def login_required(view):
    @functools.wraps(view)
    def wrapped_view(**kwargs):
        if g.user is None:
            return redirect(url_for('auth.login'))
        
        return view(**kwargs)
    
    return wrapped_view 
```

This decorator wraps the view in this way: if a user is not logged in, then redirect to login page; if logged in, return the orginal view.



### Step 6. Templates

