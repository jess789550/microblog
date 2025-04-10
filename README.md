# microblog
Flask Mega-Tutorial https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world

## How to run
```
# Activate environment that has flask and werkzeug installed
source venv/bin/activate

# Export Flask App PATH
export FLASK_APP=microblog.py

# Run app
flask run
```

## View website on local host
http://127.0.0.1:5000/

## Comparison to Django
- Nice SQL syntax (Django has it's own version of SQL)
- Django views.py and urls.py are combined into Flask routes.py
- forms.py similar
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