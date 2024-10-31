---
title: Bootstrap
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---

![bootstrap](images/bootstrap-social.png)

Bootstrap is an open-source front-end framework created for responsive web development. It created by a group of developers at Twitter and was originally referred to as Twitter Bootstrap but now common known as just "Bootstrap".  Use of the Bootstrap framework, makes it easier for developers to create webapps that look and respond correctly to a multitude of browser types on different size devices.

The code for this [version of Tweeter can be viewed on GitHub](https://github.com/cavalryjim/tweeter_v2)

## Install
Install bootstrap using pipenv.

```
(tweeter_app2) $ pipenv install django-bootstrap4
```

We need to let the Django framework know to include bootstrap.  While modifying the _settings.py_ file, go ahead and update the TEMPLATES list to look for Project-level templates.

\begin{codelisting}
\label{code:update_settings}
\codecaption{tweeter\_app/settings.py}
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'bootstrap4', # new

    'users',
    'tweets',
]

...

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [(os.path.join(BASE_DIR, 'templates')), ], # new
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
...

BOOTSTRAP4 = { 'include_jquery': True } # new

...
```
\end{codelisting}

## Project Level Templates
Soon, we will create user html templates for sign up, sign in, etc.  We want those templates to extend the base template for the overall project.  It is a good idea to create project-level templates that contain the html that all of the other templates will build on.  This will help us from repeating html code in several places.

Create a bootstrap html template that will contain all the bootstrap specific tags.

\begin{codelisting}
\label{code:bootstrap_html}
\codecaption{templates/bootstrap.html}
```
<!-- templates/bootstrap.html -->
{% extends 'bootstrap4/bootstrap4.html' %}

{% block bootstrap4_title %}{% block title %}{% endblock %}{% endblock %}
```
\end{codelisting}

This is the project-level base template that will contain the top navbar and any other common elements we need.

\begin{codelisting}
\label{code:base_html}
\codecaption{templates/base.html}
```html
<!-- templates/base.html -->
{% extends 'bootstrap.html' %}

{% load bootstrap4 %}

{% block bootstrap4_content %}
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand" href="{% url 'home' %}">Tweeter</a>
      <button class="navbar-toggler" type="button"
        data-toggle="collapse" data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item">
            <a class="nav-link" href="{% url 'tweet_new' %}">New Tweet</a>
          </li>

        </ul>

      </div>
    </nav>

    <div class="container">

        {% autoescape off %}{% bootstrap_messages %}{% endautoescape %}

        {% block content %}(no content){% endblock %}
    </div>

{% endblock %}
```
\end{codelisting}

## Update Tweets Templates
Update the tweets templates to incorporate some of the components of the bootstrap framework.

We will present the tweets as a card.

\begin{codelisting}
\label{code:home_html}
\codecaption{tweets/templates/base.html}
```html
<!-- tweets/templates/home.html -->
{% extends 'base.html' %}

{% block content %}
  {% for tweet in object_list %}
    <div class="card" >
      <div class="card-body">
        <h5 class="card-title">{{ tweet.body }}</h5>
        <p class="card-text">{{ tweet.user }}</p>
      </div>
    </div>
  {% endfor %}
{% endblock content %}
```
\end{codelisting}

Bootstrap dresses up our form for creating new tweets.

\begin{codelisting}
\label{code:home_html}
\codecaption{tweets/templates/tweet_new.html}
```html
<!-- tweets/templates/tweet_new.html -->
{% extends 'base.html' %}
{% load bootstrap4 %}
{% block content %}
  <h4>New Tweet</h4>
  <form action="" method="post">{% csrf_token %}
    {% bootstrap_form form %}
    {% buttons submit='Ok' %}{% endbuttons %}
  </form>
{% endblock content %}
```
\end{codelisting}

Now our tweets feed should look a little better.

![tweeter_bootstrap](images/tweeter_bootstrap.png)

## Function-based Views (Optional)

Here are the changes to accomplish the same results using function-based views.

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

The code for this [version of Tweeter can be viewed on GitHub](https://github.com/cavalryjim/tweeter_v2_fbv)
