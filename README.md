# microblog
Flask Mega-Tutorial https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world

## How to run
```
# Activate environment that has requirements.txt installed
source venv/bin/activate

# Export Flask App PATH
export FLASK_APP=microblog.py

# To activate debug mode
export FLASK_DEBUG=1

# Run app
flask run
```

## View website on local host
http://127.0.0.1:5000/

## Initialise database
```
flask db init
```

## Migrate database
```
flask db migrate
flask db upgrade
```

## Undo last migration
```
flask db downgrade
```

## Manipulate database through command line
```
flask shell

# Create new user
u = User(username='john', email='john@example.com')
db.session.add(u)
db.session.commit()

# Undo
db.session.rollback()

# Query database
query = sa.select(User)
users = db.session.scalars(query).all()
users

# db.session.scalars() method executes the database query and returns a results iterator
users = db.session.scalars(query)
for u in users:
    print(u.id, u.username)

# Add blog post
u = db.session.get(User, 1)
p = Post(body='my first post!', author=u)
db.session.add(p)
db.session.commit()

# get all posts written by a user
u = db.session.get(User, 1)
query = u.posts.select()
posts = db.session.scalars(query).all()
posts

# print post author and body for all posts
query = sa.select(Post)
posts = db.session.scalars(query)
for p in posts:
    print(p.id, p.author.username, p.body)

# get all users in reverse alphabetical order
query = sa.select(User).order_by(User.username.desc())
db.session.scalars(query).all()

# get all users that have usernames starting with "s"
query = sa.select(User).where(User.username.like('s%'))
db.session.scalars(query).all()
```

## Comparison to Django
- Nice SQL syntax (Django has it's own version of SQL)
- Django views.py and urls.py are combined into Flask routes.py
- forms.py and models.py similar functions
- \_\_init\_\_.py not in Django but in Flask

## Notes
- The application will exist in a package. In Python, a subdirectory that includes a \_\_init\_\_.py file is considered a package, and can be imported.
- The routes routes.py handle the different URLs that the application supports.
- To complete the application, you need to have a Python script microblog.py at the top-level that defines the Flask application instance. 
- The render_template() function invokes the Jinja template engine that comes bundled with the Flask framework. Jinja substitutes HTML {{ ... }} blocks with the corresponding values, given by the arguments provided in the render_template() call.
- HTML templates inherit from base.html
- the Flask-WTF extension is a thin wrapper around the WTForms package that nicely integrates it with Flask. 
- There are several formats for the application to specify configuration options. The most basic solution is to define your variables as keys in app.config, which uses a dictionary style to work with variables.
- The Flask-WTF extension uses Python classes to represent web forms. A form class simply defines the fields of the form as class variables.
- The form.hidden_tag() template argument generates a hidden field that includes a token that is used to protect the form against CSRF attacks.
- The form.validate_on_submit() method does all the form processing work. 
- using a "with" construct assigns the result of calling get_flashed_messages() to a messages variable, all in the context of the template.
- Flask provides a function called url_for(), which generates URLs using its internal mapping of URLs to view functions.
- Flask-SQLAlchemy, an extension that provides a Flask-friendly wrapper to the popular SQLAlchemy package, which is an Object Relational Mapper or ORM.
- Alembic (the migration framework used by Flask-Migrate) will make these schema changes in a way that does not require the database to be recreated from scratch every time a change is made.
- Alembic maintains a migration repository, which is a directory in which it stores its migration scripts
- With a regular interpreter session, the app symbol is not known unless it is explicitly imported, but when using flask shell, the command pre-imports the application instance, and pushes its application context for you.
- The app.shell_context_processor decorator registers the function as a shell context function. 