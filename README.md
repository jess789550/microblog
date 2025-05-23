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

# Use Google/MS Azure API for translation
export MS_TRANSLATOR_KEY=<paste-your-key-here>

# Run Elasticsearch with Docker
docker run --name elasticsearch -d --rm -p 9200:9200 \
    --memory="2GB" \
    -e discovery.type=single-node -e xpack.security.enabled=false \
    -t docker.elastic.co/elasticsearch/elasticsearch:8.11.1

# Run app
flask run
```

## View website on local host
http://127.0.0.1:5000/

## View ports in use
```
lsof -i4
```

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

## Run unit tests
```
python tests.py
```

## Languages and translation
```
# Add new language
flask translate init <language-code>

# Update all languages after making changes to _() and _l() language markers
flask translate update

# Compile all languages after updating translation files
flask translate compile
```

## Comparison to Django
- Nice SQL syntax (Django has it's own version of SQL)
- Django views.py and urls.py are combined into Flask routes.py
- forms.py and models.py similar functions
- \_\_init\_\_.py not in Django but in Flask

## Notes
### Chapter 1: Hello world
- The application will exist in a package. In Python, a subdirectory that includes a \_\_init\_\_.py file is considered a package, and can be imported.
- The routes routes.py handle the different URLs that the application supports.
- To complete the application, you need to have a Python script microblog.py at the top-level that defines the Flask application instance. 

### Chapter 2: Templates
- The render_template() function invokes the Jinja template engine that comes bundled with the Flask framework. Jinja substitutes HTML {{ ... }} blocks with the corresponding values, given by the arguments provided in the render_template() call.
- HTML templates inherit from base.html

### Chapter 3: Web Forms
- the Flask-WTF extension is a thin wrapper around the WTForms package that nicely integrates it with Flask. 
- There are several formats for the application to specify configuration options. The most basic solution is to define your variables as keys in app.config, which uses a dictionary style to work with variables.
- The Flask-WTF extension uses Python classes to represent web forms. A form class simply defines the fields of the form as class variables.
- The form.hidden_tag() template argument generates a hidden field that includes a token that is used to protect the form against CSRF attacks.
- The form.validate_on_submit() method does all the form processing work. 
- using a "with" construct assigns the result of calling get_flashed_messages() to a messages variable, all in the context of the template.
- Flask provides a function called url_for(), which generates URLs using its internal mapping of URLs to view functions.

### Chapter 4: Database
- Flask-SQLAlchemy, an extension that provides a Flask-friendly wrapper to the popular SQLAlchemy package, which is an Object Relational Mapper or ORM.
- Alembic (the migration framework used by Flask-Migrate) will make these schema changes in a way that does not require the database to be recreated from scratch every time a change is made.
- Alembic maintains a migration repository, which is a directory in which it stores its migration scripts
- With a regular interpreter session, the app symbol is not known unless it is explicitly imported, but when using flask shell, the command pre-imports the application instance, and pushes its application context for you.
- The app.shell_context_processor decorator registers the function as a shell context function. 

### Chapter 5: User Logins
- One of the packages that implement password hashing is Werkzeug
- Flask extension Flask-Login manages the user logged-in state, so that for example users can log in to the application and then navigate to different pages while the application "remembers" that the user is logged in.
- Flask-Login keeps track of the logged in user by storing its unique identifier in Flask's user session, a storage space assigned to each user who connects to the application. 
-  If a user who is not logged in tries to view a protected page, Flask-Login will automatically redirect the user to the login form, and only redirect back to the page the user wanted to view after the login process is complete.

### Chapter 6: Profile Page and Avatars
- Gravatar is great for avatars: https://gravatar.com/
- You can use Jinja sub-templates using the _ prefix for a HTML template

### Chapter 7: Error Handling
- You can email errors automatically (see config.py)
- The RotatingFileHandler class is nice because it rotates the logs, ensuring that the log files do not grow too large when the application runs for a long time. In this case I'm limiting the size of the log file to 10KB
- Logging categories: DEBUG, INFO, WARNING, ERROR and CRITICAL

### Chapter 8: Followers
- SQL DB relationships: one-to-many, many-to-many, one-to-one, many-to-one

### Chapter 9: Pagination
- Flask-SQLAlchemy supports pagination natively with the db.paginate() function, which works like db.session.scalars(), but with pagination built-in.

### Chapter 10: Email Support
- A popular token standard for this password reset is the JSON Web Token, or JWT. The nice thing about JWTs is that they are self-contained. You can send a token to a user in an email, and when the user clicks the link that feeds the token back into the application, it can be verified on its own.
- Python has support for running asynchronous tasks, actually in more than one way. The threading and multiprocessing modules can both do this.

### Chapter 11: Facelift
- A CSS framework provides a collection of high-level CSS classes with pre-made styles for common types of user interface elements. 
- One of the most popular CSS frameworks is Bootstrap.
- The most direct way to use Bootstrap is to simply import the bootstrap.min.css file in your base template. You can either download a copy of this file and add it to your project, or import it directly from a Content Delivery Network.

### Chapter 12: Dates and Times
- As it turns out, the web browser knows the user's timezone, and exposes it through the standard date and time JavaScript APIs. There are actually two ways to take advantage of the timezone information available via JavaScript:
    - The "old school" approach would be to have the web browser somehow send the timezone information to the server when the user first logs in to the application. This could be done with an Ajax call, or much more simply with a meta refresh tag. Once the server knows the timezone it can keep it in the user's session or write it to the users table in the database, and from then on adjust all timestamps with it at the time templates are rendered.
    - The "new school" approach would be to not change a thing in the server, and let the conversion from UTC to local timezone happen in the browser, using JavaScript.
- Moment.js is a small open-source JavaScript library that takes date and time rendering to another level, as it provides every imaginable formatting option, and then some.
    - Moment.js makes a moment class available to the browser. The first step to render a timestamp is to create an object of this class, passing the desired timestamp in ISO 8601 format: {yyyy}-{mm}-{dd}T{hh}:{mm}:{ss}{tz}

### Chapter 13: I18n and L10n
- Internationalization and Localization, commonly abbreviated I18n and L10n
- Translation into other languages using Flask-Babel
- Once you have the application with all the _() and _l() in place, you can use the pybabel command to extract them to a .pot file, which stands for portable object template.
- The way texts are marked for translation is by wrapping them in a function call that as a convention is called _(), just an underscore. 
- Flask-Babel provides a lazy evaluation version of _() that is called lazy_gettext() 

### Chapter 14: Ajax
- Server-side: HTTP requests give HTML page
- Client-side: HTML + JavaScript page
- A single-page application (SPA) is a web application or website that interacts with the user by dynamically rewriting the current web page with new data from the web server, instead of the default method of loading entire new pages.
- To do real time translations of user posts, the client browser will send asynchronous requests to the server, to which the server will respond without causing a page refresh.
- The client will then insert the translations into the current page dynamically. 
- This technique is known as Ajax, which is short for Asynchronous JavaScript and XML (even though these days XML is often replaced with JSON).

### Chapter 15: Better Application Structure
- In Flask, a blueprint is a logical structure that represents a subset of the application. A blueprint can include elements such as routes, view functions, forms, templates and static files.
- The contents of a blueprint are initially in a dormant state. To activate these elements, the blueprint needs to be registered with the application.

### Chapter 16: Full Text Search
- There are several open-source full-text engines: Elasticsearch, Apache Solr, Whoosh, Xapian, Sphinx, etc.
- Elasticsearch is one that stands out to me as being fairly popular, in part due to its popularity as the "E" in the ELK stack for indexing logs, along with Logstash and Kibana.