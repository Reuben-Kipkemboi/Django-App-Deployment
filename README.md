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














