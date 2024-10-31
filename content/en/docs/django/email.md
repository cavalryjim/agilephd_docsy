---
title: Email
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 10
---


![email](images/email.png)

Adding email to an application allows us to provide a better user experience.  Users have come to expect certain features such as account confirmation and password reset which are usually accomplished via email.

A version of this code can be viewed on [GitHub](https://github.com/cavalryjim/tweeter_v5).

## SendGrid

You could acquire email services through dozens of reputable providers but one of the easiest, and free-ist, methods is to use the Sendgrid add-on via Heroku.

```
(tweeter_app) $ heroku addons:create sendgrid:starter
Creating sendgrid:starter on ⬢ secret-river-48719... free
...
(tweeter_app) $ heroku config:get SENDGRID_USERNAME
app161585718@heroku.com
(tweeter_app) $ heroku config:get SENDGRID_PASSWORD
90e3xxxxxxxx

```

Make a note of the SENDGRID_USERNAME and SENDGRID_PASSWORD as we will need those when setting up the email server connection.

Edit the _tweeter_app/settings.py_ file to define our email service.

\begin{codelisting}
\label{code:update_settings}
\codecaption{tweeter\_app/settings.py}
```python
...

LOGIN_REDIRECT_URL = 'home'
LOGOUT_REDIRECT_URL = 'home'

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend' # new
EMAIL_HOST = 'smtp.gmail.com' # new
EMAIL_HOST_USER =  'xxxxx@gmail' # new
EMAIL_HOST_PASSWORD = 'xxxxxxxxxx' # new
EMAIL_PORT = 587 # new
EMAIL_USE_TLS = True # new

...
```
\end{codelisting}

Note that you will use your email provider EMAIL_HOST_USER and EMAIL_HOST_PASSWORD.  Also note that it is a really, really bad practice to openly display password or API keys directly in source code.  There are people that scan public repositories for this information and use it for Spamming or other bad things.  Later in this chapter, I will cover how store your secrets in environmental variables.

Start the development server.

```
(tweeter_app) $ python manage.py runserver
```

Let's see if our email connection is working by logging out (if you are logged in) and trying the password reset feature.

![password reset page](images/reset_password_screen.png)

If everything is working, you should have received an email from
webmaster@localhost.

## Environmental Variables
There are several methods for handling environmental variables.  Here is the method most commonly endorsed by the Django community.

```
$ pip install django-environ
```

\begin{codelisting}
\label{code:update_settings}
\codecaption{tweeter\_app/settings.py}
```python
import os
import django_heroku
import environ # new

env = environ.Env() # new

# reading .env file
environ.Env.read_env() # new

...

EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_HOST_USER =  env.str('GMAIL_USERNAME') # new
EMAIL_HOST_PASSWORD = env.str('GMAIL_PASSWORD') # new
EMAIL_PORT = 587
EMAIL_USE_TLS = True

...
```
\end{codelisting}

For our local environmental variable, create a _.env_ file inside the base folder of your tweeter project.  Inside of the _.env_ file, add your sendgrid credentials.

\begin{codelisting}
\label{code:hidden_env}
\codecaption{.env}
```
GMAIL_USERNAME='xxxxxxxx@gmail.com'
GMAIL_PASSWORD='yupxxxxxxxxxxxx'
```
\end{codelisting}

## Git Ignore
Now that we have created a _.env_ file, it would be a good opportunity to define a _.gitignore_ file.  This file specifies   files that will not be tracked as part of our Git repository.  Thus, this list of files will be ignored.

\begin{codelisting}
\label{code:gitignore}
\codecaption{.gitignore}
```
staticfiles
.env
db.sqlite3
```
\end{codelisting}
