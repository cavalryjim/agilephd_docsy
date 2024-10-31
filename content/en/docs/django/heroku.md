---
title: Heroku
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---

![Containers](images/heroku_cloud.png)

**Heroku** is a platform as a service (PaaS) that enables developers to build, run, and operate applications entirely in the cloud. Heroku just works and makes deploying applications easy. The pricing structure makes it very cost effective for development, prototyping, and deploying a minimal viable product (MVP).   

## Setup account
[Heroku](https://signup.heroku.com/) uses a Freemium pricing model.  The basic level account is free so establish your account.

## Heroku CLI
The Heroku Command Line Interface (CLI), also known as the Heroku Toolbelt, is a tool for creating and managing Heroku apps from the command line.

### MacOS
Install Heroku CLI using homebrew.  If needed, read the [Heroku CLI install instructions](https://devcenter.heroku.com/articles/heroku-cli).

```
$ brew tap heroku/brew && brew install heroku
```

### Windows
Download and run the [Windows installer](https://devcenter.heroku.com/articles/heroku-cli).

### Verify Your Installation
To verify your CLI installation, use the heroku --version command:

```
(django-pages) $ heroku --version
heroku/7.25.1 (darwin-x64) node-v8.0.0
```

You should see heroku/x.y.z in the output.

### Heroku Login
Once installation is complete make sure you are in the directory with the pages virtual environment active.
Type the command _heroku login_ and use the email and password for Heroku you just created.

```
(django-pages) $ heroku login
Enter your Heroku credentials:
Email: james.davis@gmail.com
Password: *********************************
Logged in as james.davis@gmail.com
```

## Update Pipfile
Heroku needs to know what version of Python to use for this application.  Add these lines to the bottom of your _Pipfle_.

\begin{codelisting}
\label{code:pipfile}
\codecaption{Pipfile}
```
...
[requires]
python_version = "3.9"
```
\end{codelisting}

Run _pipenv lock_ to generate the appropriate _Pipfile.lock_.

```
(django-pages) $ pipenv lock
```

Heroku used the _Pipfile.lock_ for information about the version of Python we are using and packages to install.

## Procfile
Another thing Heroku wants is a _Procfile_. This tells Heroku which commands to run in order to start our web application. Open up your code editor, create a file called _Procfile_ in the root directory of your project.
\begin{codelisting}
\label{code:procfile}
\codecaption{Procfile}
```python
web: gunicorn django_pages.wsgi
```
\end{codelisting}

Gunicorn is a robust web server that will be used for our production version of the app.  The built-in Django server is only suitable for local development.  Install gunicorn using Pipenv.

```
(django-pages) $ pipenv install gunicorn
```

## settings.py
Update ALLOWED_HOSTS in the _settings.py_ file.

\begin{codelisting}
\label{code:procfile}
\codecaption{django\_pages/settings.py}
```python
...
ALLOWED_HOSTS = ['*']
...
```
\end{codelisting}

In a production-level Django site we would explicitly list which domains were allowed but not for this project.

## Push to GitHub
It is good practice to regularly push changes to GitHub.

```
(django_pages) $ git add .
(django_pages) $ git commit -m "configuring for heroku"
(django_pages) $ git push
```

## Push to Heroku
From the earlier Heroku setup, you should already have a Heroku account that let's you create new apps.
```
(django-pages) $ heroku create
```

At the moment, tell Heroku to ignore static files like CSS and JavaScript.  We will cover this in later chapters so for now just run the following command.
```
(django-pages) $ heroku config:set DISABLE_COLLECTSTATIC=1
```

Push the code to Heroku.
```
(django-pages) $ git push heroku
```

Now, you can either navigate to your production application or use _heroku open_.
```
(django-pages) $ heroku open
```

Celebrate!  You are now have deployed a production application.
