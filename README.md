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

```sh
pip install gunicorn
```
and to add it to your requirements file

```sh
pip freeze > requirements.txt
```
*key note gunicorn is the file that declares how the applications should run, don't forget to have it installed*

* A `runtime.txt` file in the project root.This basically tells the python version being used.

* To serve your static files, You will require to configure `whitenoise`.

* ` .env` - We will use this to store our config variables and have it in our `.gitignore`

`.gitignore` - when committing our variables will be ignored

## Steps to Deploy

# Login

* Sign in to your [Heroku Account](https://dashboard.heroku.com/).

* Install the [Heroku Toolbelt](https://devcenter.heroku.com/articles/heroku-cli).

It is  a package of the Heroku CLI, Foreman, and Git — all the tools you need to get started using Heroku at the command line.

* Now login to your heroku account via the terminal or Command-line interface you are using by running;

```sh
heroku login
```
You will be redirected to heroku login to confirm the login and once successful you will login to your account successfully.

Login screenshot


# Procfile

Procfile is a file that instructs of  a mechanism for declaring what commands are run by your application.The `Procfile` should be added to the applications root directory.

Create a file with the name `Procfile` in your project's Root directory and inside the file have;

```sh
web: gunicorn <your_project_name>.wsgi
```

*project name is dynamic so have it replaced with the name of the project for instance if my project name is tuesday the procfile contents should be ;*

```sh
web: gunicorn tuesday.wsgi
```

# Runtime.txt

To specify a Python runtime, add a `runtime.txt` file to your app’s root directory that declares the exact version number to use:

To check the python version.
```sh
python --version 
```
or

```sh
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
```sh
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

```sh
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

```sh
# configuring the location for media
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

```

Much on [WhiteNoise](http://whitenoise.evans.io/en/stable/#:~:text=Radically%20simplified%20static%20file%20serving,or%20any%20other%20external%20service.)

# Requirements.txt

If your under a virtual environment run the command below to generate the requirements.txt file which heroku will use to install python package dependencies.

`pip freeze > requirements.txt` this adds all the dependancies are in thr requirements.txt

A sample requirements.txt file

```sh
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

# Database configurations

[Decouple](https://pypi.org/project/python-decouple/) helps you to organize your settings so that you can change parameters without having to redeploy your app.

It makes it easy:
1. Store parameters in ini or .env files;
2. Define comprehensive default values;
3. Properly convert values to the correct data type;
4. Have only one configuration module to rule all your instances.

Installations;
```sh
pip install python-decouple
```

## dj-database-url

This simple Django utility allows you to utilize the 12factor inspired DATABASE_URL environment variable to configure your Django application.

Installations;
```
pip install dj-database-url
```
[dj-database-url](https://pypi.org/project/dj-database-url/)


Our `.env` file sample

```sh
#Just an example, don't share your .env settings
SECRET_KEY='myseceretkeyalwayssecretaswell0101'
DEBUG=True
DB_NAME='database'
DB_USER='user'
DB_PASSWORD='password'
DB_HOST='127.0.0.1'
MODE='dev'
ALLOWED_HOSTS='<app name in heroku>.herokuapp.com'
DISABLE_COLLECTSTATIC=1
```

**Don't share your settings**

Now that we have our variables in the`.env` file , we can edit the settings to use our configurations

```sh

import os #operating system
import django_heroku
import dj_database_url
from decouple import config,Csv

MODE=config("MODE", default="dev")
SECRET_KEY = config('SECRET_KEY')
DEBUG = config('DEBUG', default=False, cast=bool)

# development
if config('MODE')=="dev":
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.postgresql_psycopg2',
           'NAME': config('DB_NAME'),
           'USER': config('DB_USER'),
           'PASSWORD': config('DB_PASSWORD'),
           'HOST': config('DB_HOST'),
           'PORT': '',
       }
       
   }
# production
else:
   DATABASES = {
       'default': dj_database_url.config(
           default=config('DATABASE_URL')
       )
   }

db_from_env = dj_database_url.config(conn_max_age=500)
DATABASES['default'].update(db_from_env)

ALLOWED_HOSTS = config('ALLOWED_HOSTS', cast=Csv())


```

# Deployment
























