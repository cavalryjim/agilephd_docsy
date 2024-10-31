---
title: Tweeter (Twitter-ish) App
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---

![admin login](images/twitter.png)

For our next app, we will build a very basic Twitter-ish appliation.  This app will introduce the CustomUser model, Admin module, Migrations, and much more.

## Project Plan
Here are the initial steps that we will follow for this creating the first version of Tweeter:

- create a _tweeter_app_ directory
- install Django using Pipenv
- create a new Django project
- create a _users_ app
- create a _tweets_ app
- configure admin module
- setup urls / views / templates
- add tests
- deploy to Heroku

The code for this [version of Tweeter can be viewed on GitHub](https://github.com/cavalryjim/tweeter_v1)

## Getting Started

```
$ cd Projects
$ mkdir tweeter_app
$ cd tweeter_app
$ pwd
/Users/james/Projects/tweeter_app
```

Use Pipenv to create a new virtual environment, install Django, and activate the environment.

```
$ pipenv install django
...
$ pipenv shell
...
(tweeter_app) $ django-admin startproject tweeter_app .
```

In the _users/models.py_ file, we are going to use a package to help compute a users age from their date of birth.  You may need to install the _python-dateutil_ package.
```
(tweeter_app) $ pipenv install python-dateutil
```

## Databases
A database is at the heart of any good application.  The database provides an efficient storage layer for our data.  Django comes with a robust ORM (Object-Relational Mapper) that provides built-in support for most of the popular database management systems (DBMS) to include: PostgreSQL, MySQL, MariaDB, Oracle, or SQLite. For the developer, it allows you to create a Python class, called a model, that will automatically be translated correctly regardless of the type of database.

This is one of the main reasons to use a framework such as Django.  Otherwise, you would spend time & effort on the nuanced differences between DBMS types.  

By default, Django uses SQLite for the local development environment.  SQLite is lightweight, portable, and does not require complex installation. By contrast, most other databases require a dedicated server separate from Django itself, which is beyond the current scope of this project.

This project will use SQLite for our local database. Our production version, hosted on Heroku, will use PostgreSQL.

## CustomUser
Django comes with a built-in User model that allows you to immediately working with users. However, the Django community, and [official Django documentation](https://docs.djangoproject.com/en/3.0/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project), _highly recommends_ using a custom user model.  The reasoning behind this recommendation is that is makes it much easier to make changes to the User model if you start with a CustomUser.
Bottom line: __Always create a custom user model.__

Before creating a CustomUser model, review the fields & attributes that come with the default [Django User model](https://docs.djangoproject.com/en/3.0/ref/contrib/auth/#django.contrib.auth.models.User).

## User App
We will start by creating a User app that will allow us to create the CustomUser model.

```
(tweeter_app) $ python manage.py startapp users
```

Update _tweeter_app/settings.py_.  Add 'users' to the list of installed apps and set AUTH_USER_MODEL letting Django know we are using a custom user model.

\begin{codelisting}
\label{code:add_users}
\codecaption{tweeter\_app/settings.py}
```python
...
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users', # new
]

AUTH_USER_MODEL = 'users.CustomUser' # new
...
```
\end{codelisting}

By default, the Django User model comes with several common attributes (username, first_name, etc.) that we will use.  For our CustomUser model, let's add gender, dob, and location.  Eventually, we want to restrict the age of our users so let's go ahead and add an _age_ function along with a _ __str__ _ function

\begin{codelisting}
\label{code:users_model}
\codecaption{users/models.py}
```python
from django.db import models
from django.contrib.auth.models import AbstractUser
from datetime import date
from dateutil.relativedelta import relativedelta

class CustomUser(AbstractUser):
    # These are the fields we want on top of the fields included
    #  with the built-in Django User Model that come with:
    #  username, first_name, last_name, email, password, ....
    gender = models.CharField(max_length=255, blank=True, null=True)
    dob = models.DateField(null=True, blank=True)
    location = models.CharField(max_length=255, blank=True, null=True)

    def __str__(self):
        return self.username

    def age(self):
        if self.dob == None:
            return None
        age = relativedelta(date.today(), self.dob)
        return age.years
```
\end{codelisting}

Create migrations and migrate the database.
```
(tweeter_app) $ python manage.py makemigrations
...
(tweeter_app) $ python manage.py migrate
...
```

## Tweets App
Next, create a _Tweets_ app.

```
(tweeter_app) $ python manage.py startapp tweets
```

Same as before, add 'tweets' to the list of installed apps in _tweeter_app/settings.py_.

\begin{codelisting}
\label{code:add_tweets}
\codecaption{tweeter\_app/settings.py}
```python
...
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users',
    'tweets', # new
]
...
```
\end{codelisting}

Define the Tweet data model.

\begin{codelisting}
\label{code:tweets_model}
\codecaption{tweets/models.py}
```python
from django.db import models
from django.contrib.auth import get_user_model
from django.utils import timezone

class Tweet(models.Model):
    user = models.ForeignKey(get_user_model(), on_delete=models.PROTECT)
    body = models.CharField(max_length=255)

    created_at = models.DateTimeField(default=timezone.now)
    updated_at = models.DateTimeField(default=timezone.now)

    def __str__(self):
        return self.body
```
\end{codelisting}

Create migrations and migrate the database.
```
(tweeter_app) $ python manage.py makemigrations
...
(tweeter_app) $ python manage.py migrate
...
```

## Django Admin
Applications of any size or complexity require some level of administration.  App administrators need the ability to create, read, update, and delete data from any of the models. For that reason, Django comes with a robust admin interface.

To use the Django admin, it does require a bit of configuration. First we’ll need to create a user who can login to the admin site. Run the following command:
```
(tweeter_app) $ python manage.py createsuperuser
```
Enter your desired username and press enter.
```
Username: admin
```
You will then be prompted for your desired email address:
```
Email address: admin@example.com
```
The final step is to enter your password. You will be asked to enter your password twice, the second time as a confirmation of the first.
```
Password: **********
Password (again): *********
Superuser created successfully.
```
Modify the _tweets/admin.py_ file to add the Tweet model as part of the admin module.

\begin{codelisting}
\label{code:tweets_admin}
\codecaption{tweets/admin.py}
```python
from django.contrib import admin
from .models import Tweet

admin.site.register(Tweet)
```
\end{codelisting}

Now modify the _users/admin.py_ file to add the CustomUser model as part of the admin module.

\begin{codelisting}
\label{code:users_admin}
\codecaption{users/admin.py}
```python
from django.contrib import admin
from .models import CustomUser

admin.site.register(CustomUser)
```
\end{codelisting}

Time to look at our Admin interface. If it isn't already running, start the development web server.
```
(tweeter_app) $ python manage.py runserver
```
Select this [link](http://127.0.0.1:8000/admin/) or go to your browser and type the address http://127.0.0.1:8000/admin/. You will see a login page like this:
![admin login](images/admin_login.png)

Login using your superuser credentials and you should see the Django admin dashboard.  Go to Tweets and add a couple "tweets".

## Urls / Views / Templates for Tweets
While we could do all of our content creation from the admin module, it is only intended to be used by the actual application administrators.  We will create a couple pages for users to create and view tweets.

Update the _tweeter_app/urls.py_ to include the tweets.urls.

\begin{codelisting}
\label{code:tweeter_urls}
\codecaption{tweeter\_app/urls.py}
```python
# tweeter_app/urls.py
from django.contrib import admin
from django.urls import path, include # new

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tweets.urls')), # new
]
```
\end{codelisting}

In the _tweets/urls.py_ file, route traffic to the appropriate class (or function) in the views.py file.

\begin{codelisting}
\label{code:tweets_urls}
\codecaption{tweets/urls.py}
```python
# tweets/urls.py
from django.urls import path
from .views import TweetListView, TweetCreateView

urlpatterns = [
    path('tweets/new/', TweetCreateView.as_view(), name='tweet_new'),
    path('', TweetListView.as_view(), name='home'),
]
```
\end{codelisting}

Now, create the appropriate classes in the _tweets/views.py_ file.

\begin{codelisting}
\label{code:tweets_views}
\codecaption{tweets/views.py}
```python
# tweets/views.py
from django.views.generic import ListView
from django.views.generic.edit import CreateView
from django.urls import reverse
from .models import Tweet

class TweetListView(ListView):
    model = Tweet
    template_name = 'home.html'

class TweetCreateView(CreateView):
    model = Tweet
    template_name = 'tweet_new.html'
    fields = ['user', 'body']

    def get_success_url(self):
        return reverse('home')
```
\end{codelisting}

We will create three templates.  A base template that will provide the basic html structure.  A home template that will display all the tweets.  Lastly, a template for creating new tweets.

\begin{codelisting}
\label{code:tweets_base}
\codecaption{tweets/templates/base.html}
```html
<!-- tweets/templates/base.html -->
<html>
  <head>
    <title>Tweeter</title>
  </head>
  <body>
    <header>
      <div class="nav-left">
        <h1><a href="{% url 'home' %}">Tweeter</a></h1>
      </div>
      <div class="nav-right">
        <a href="{% url 'tweet_new' %}">Add Tweet</a>
      </div>
    </header>

    <div>
    {% block content %} {% endblock content %}
    </div>
  </body>
</html>
```
\end{codelisting}

\begin{codelisting}
\label{code:tweets_home}
\codecaption{tweets/templates/home.html}
```html
<!-- tweets/templates/home.html -->
{% extends 'base.html' %}

{% block content %}
  {% for tweet in object_list %}
    <div class="tweet-entry">
    <h4>{{ tweet.body }}</h4>
    <p>{{ tweet.user }}</p>
    </div>
  {% endfor %}
{% endblock content %}
```
\end{codelisting}

\begin{codelisting}
\label{code:tweets_new}
\codecaption{tweets/templates/tweet\_new.html}
```html
{% extends 'base.html' %}

{% block content %}
  <h1>New Tweet</h1>
  <form action="" method="post">{% csrf_token %}
    {{ form.as_p }}
    <input type="submit" value="Save" />
  </form>
{% endblock content %}
```
\end{codelisting}

## Tests
For completeness (and good practice), we create a test suite for the tweets app.

\begin{codelisting}
\label{code:tweets_tests}
\codecaption{tweets/tests.py}
```python
# tweets/tests.py
from django.contrib.auth import get_user_model
from django.test import TestCase
from django.urls import reverse
from .models import Tweet

class TweetTests(TestCase):
    def setUp(self):
        self.user = get_user_model().objects.create_user(
            username='testuser',
            email='test@email.com',
            password='specialpwd'
        )

        self.tweet = Tweet.objects.create(
            body='Nice tweet!',
            user=self.user,
        )

    def test_tweet_string(self):
        tweet = Tweet(body='A sample tweet')
        self.assertEqual(str(tweet), tweet.body)

    def test_tweet_content(self):
        self.assertEqual(f'{self.tweet.user}', 'testuser')
        self.assertEqual(f'{self.tweet.body}', 'Nice tweet!')

    def test_tweet_list_view(self):
        response = self.client.get(reverse('home'))
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'Nice tweet!')
        self.assertTemplateUsed(response, 'home.html')

    def test_tweet_create_view(self):
        response = self.client.post(reverse('tweet_new'), {
            'body': 'New tweet',
            'user': self.user,
        })
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'New tweet')
```
\end{codelisting}

If all is working, you should be able to run your test suite.
```
(tweeter_app) $ python manage.py test
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
....
----------------------------------------------------------------------
Ran 4 tests in 0.748s

OK
Destroying test database for alias 'default'...
```

The code for this [version of Tweeter can be viewed on GitHub](https://github.com/cavalryjim/tweeter_v1)

If you haven't done so already, this is a good opportunity to create a git repository and push to GitHub.
```
(tweeter_app) $ git init
(tweeter_app) $ git add .
(tweeter_app) $ git remote add origin https://github.com/cavalryjim/tweeter_app.git
(tweeter_app) $ git push -u origin master
```

## Heroku
Let's configure for and deploy the app to Heroku.  Add these lines to the bottom of your _Pipfle_.

\begin{codelisting}
\label{code:pipfile}
\codecaption{Pipfile}
```
...
[requires]
python_version = "3.7"
```
\end{codelisting}

Run _pipenv lock_ to generate the appropriate _Pipfile.lock_.

```
(tweeter_app) $ pipenv lock
```

Open up your code editor, create a file called _Procfile_ in the root directory of your project.
\begin{codelisting}
\label{code:procfile}
\codecaption{Procfile}
```python
web: gunicorn tweeter_app.wsgi
```
\end{codelisting}

Install gunicorn and django_heroku using Pipenv.
```
(tweeter_app) $ pipenv install gunicorn
(tweeter_app) $ pipenv install django_heroku
```

Update the _settings.py_ file for Heroku.

\begin{codelisting}
\label{code:procfile}
\codecaption{tweeter\_app/settings.py}
```python
import os
import django_heroku # <- this line goes near the top of the file

...
ALLOWED_HOSTS = ['*'] # <-- new.
...

# Activate Django-Heroku.
django_heroku.settings(locals()) # <- this line should be a the end of the file
```
\end{codelisting}

Push your updated code to GitHub.
```
(tweeter_app) $ git add .
(tweeter_app) $ git commit -m "configuring for heroku"
(tweeter_app) $ git push
```

You might need to run 'heroku login' which will allow you to create a new heroku app and push your code to your production version on Heroku.
```
(tweeter_app) $ heroku login
(tweeter_app) $ heroku create
(tweeter_app) $ git push heroku master
```

We need to migrate the production database on Heroku and create a superuser for the admin module.
```
(tweeter_app) $ heroku run python manage.py migrate
(tweeter_app) $ heroku run python manage.py createsuperuser
```

Open and view your app on Heroku.
```
(tweeter_app) $ heroku open
```

Celebrate!

## (Optional) Function-based Views

Here are the changes to accomplish the same results as above using function-based views.

\begin{codelisting}
\label{code:tweeter_urls_fbv}
\codecaption{tweeter\_app/urls.py}
```python
# tweets/urls.py
from django.urls import path
# from .views import TweetListView, TweetCreateView
from . import views

urlpatterns = [
    path('tweets/new/', views.tweet_new, name='tweet_new'),
    path('', views.tweet_list, name='home'),

    # path('tweets/new/', TweetCreateView.as_view(), name='tweet_new'),
    # path('', TweetListView.as_view(), name='home'),
]
```
\end{codelisting}

\begin{codelisting}
\label{code:tweets_views_fbv}
\codecaption{tweets/views.py}
```python
# tweets/views.py
from django.views.generic import ListView
from django.views.generic.edit import CreateView
from django.urls import reverse
from django.shortcuts import render, redirect
from .models import Tweet
from .forms import TweetForm

def tweet_list(request):
    return render(request, 'home.html', {'object_list': Tweet.objects.all()})

def tweet_new(request):
    if request.method == 'POST':
        form = TweetForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('home')
    else:
        form = TweetForm

    return render(request, 'tweet_new.html', {'form': form })
```
\end{codelisting}

And you will need to add a form that will be used to submit the new tweets.

\begin{codelisting}
\label{code:tweets_forms_fbv}
\codecaption{tweets/forms.py}
```python
from django import forms
from .models import Tweet

class TweetForm(forms.ModelForm):

    class Meta:
        model = Tweet
        fields = ['user', 'body']
```
\end{codelisting}
