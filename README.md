# Deploy your Django application to Heroku.

## Requirements

*My assumptions are: You have a django app that is linked to a postgres database*

* Heroku account.[Create one](https://dashboard.heroku.com/)
* A simple django app to deploy.
* Application is configured to a postgres database 

## File requirements

* `Procfile` - Procfile is a file that instructs of  a mechanism for declaring what commands are run by your application.The `Procfile` should be added to the applications root directory.

* `Requirements.txt` file which is also added to the root of the application .The file is a type of file that usually stores information about all the libraries, modules, and packages in itself that are used while developing a particular project...

* Install `gunicorn` and add it to your requirements.txt file simply by running the following command 

```
pip install gunicorn
```
and to add it to your requirements file

```
pip freeze > requirements.txt
```
*key note gunicorn is the file that declares how the applications should run, don't forget to have it installed*

* A `runtime.txt` file in the project root.This basically tells the python version being used.

* To serve your static files, You will require to configure `whitenoise`.

## Steps to Deploy

# Login

* Sign in to your [Heroku Account](https://dashboard.heroku.com/).

* Install the [Heroku Toolbelt](https://devcenter.heroku.com/articles/heroku-cli).

It is  a package of the Heroku CLI, Foreman, and Git — all the tools you need to get started using Heroku at the command line.

* Now login to your heroku account via the terminal or Command-line interface you are using by running;

```
heroku login
```
You will be redirected to heroku login to confirm the login and once successful you will login to your account successfully.

Login screenshot


# Procfile

Procfile is a file that instructs of  a mechanism for declaring what commands are run by your application.The `Procfile` should be added to the applications root directory.

Create a file with the name `Procfile` in your project's Root directory and inside the file have;

```
web: gunicorn <your_project_name>.wsgi
```

*project name is dynamic so have it replaced with the name of the project for instance if my project name is tuesday the procfile contents should be ;*

```
web: gunicorn tuesday.wsgi
```

# Runtime.txt

To specify a Python runtime, add a `runtime.txt` file to your app’s root directory that declares the exact version number to use:

To check the python version.
```
python --version or
```

```
python -V
```

on your ` runtime.txt ` 

```
python-3.8.10
```

# Configure Django Heroku

Django Heroku is a Django library for Heroku applications that ensures a seamless deployment and development experience.

This library provides;

* Settings configuration (Static files / WhiteNoise).
* Logging configuration.
* Test runner

To install
```
pip install django-heroku   
```
*remember to add it to your requirements.txt file by `pip freeeze > requirements.txt`*

# Whitenoise configuration

With a couple of lines of config WhiteNoise allows your web app to serve its own static files, making it a self-contained unit that can be deployed anywhere without relying on nginx, Amazon S3 or any other external service.

* Installation

```
pip install whitenoise
```

* In your `settings.py` file add whitenoise to the `middleware` list
above all other middleware apart from Django’s SecurityMiddleware:

```
MIDDLEWARE = [
    # ...
    "django.middleware.security.SecurityMiddleware",
    "whitenoise.middleware.WhiteNoiseMiddleware",
    # ...
]
```
Want forever-cacheable files and compression support? Just add this to your `settings.py`(We basically want to give a gzip functionality):

```
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"
```

*configuring your location for media* 

```
# configuring the location for media
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

```

Much on [WhiteNoise](http://whitenoise.evans.io/en/stable/#:~:text=Radically%20simplified%20static%20file%20serving,or%20any%20other%20external%20service.)

# Requirements.txt

If your under a virtual environment run the command below to generate the requirements.txt file which heroku will use to install python package dependencies.

`pip freeze > requirements.txt` this adds all the dependancies are in thr requirements.txt

A sample requirements.txt file

```
asgiref==3.5.2
backports.zoneinfo==0.2.1
beautifulsoup4==4.11.1
bootstrap4==0.1.0
certifi==2022.5.18.1
cloudinary==1.29.0
dj-database-url==0.5.0
Django==4.0.5
django-bootstrap4==22.1
django-heroku==0.3.1
djangorestframework==3.13.1
gunicorn==20.1.0
psycopg2==2.9.3
python-decouple==3.6
pytz==2022.1
six==1.16.0
soupsieve==2.3.2.post1
sqlparse==0.4.2
urllib3==1.26.9
whitenoise==6.2.0
```
















