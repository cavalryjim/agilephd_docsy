---
title: Static Pages Application
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---

For our first application, we will create and deploy a basic Django app that serves some static html pages.  This exercise will introduce us Django Projects vs Applications.  We will also deploy the application on a production hosting service called Heroku.

## Project Plan
Here are the steps that we will follow for this creating this application:

- create a _django_pages_ directory
- install Django using Pipenv
- create a new Django project
- create a new pages app
- update _settings.py_
- create _base.html_, _home.html_, & _about.html_ templates
- create appropriate _urls.py_ routes
- update _views.py_
- add a tests
- deploy to Heroku

## Getting Started

```
$ mkdir django_pages
$ cd django_pages
$ pwd
/Users/james/Projects/django_pages
```

The _pwd_ command is short for 'print working directory' and displays the complete path for the current directory location.  The command works on Mac, Linux, and Unix operating systems.  Windows users can use the _dir_ command to see similar information.

Use Pipenv to create a new virtual environment, install Django, and activate the environment.

```
$ pipenv install django
...
$ pipenv shell
```

## Create Django Project

```
(django_pages)$ django-admin startproject django_pages .
(django_pages)$ python manage.py startapp pages
```

\begin{aside}
\label{aside:project_v_app}
\heading{Projects vs apps}

\noindent
What’s the difference between a project and an app? An app is a Web application that does something – e.g., maintain a blog, conduct a poll, maintain user profiles. A project is a collection of configuration and apps for a particular website. A project can, and usually does, contain multiple apps.
\end{aside}

Django automatically generates the basic file structure of an app, so you can focus on writing code rather than creating directories.
```
pages/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```
After creating an application, we also need to tell Django that it should use it. We do that in the file _django_pages/settings.py_. We need to find INSTALLED_APPS and add a line containing 'blog' above the closing bracket. So the final product should look like this:

\begin{codelisting}
\label{code:add_app}
\codecaption{django\_pages/settings.py}
```python
...
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'pages' # new
]
...
```
\end{codelisting}

As a check that everything is working, start the development web server with _runserver_.

```
(django_pages) $ python manage.py runserver
```

And navigate to: [localhost:8000](http://localhost:8000).

![Success](images/install_successfully.png)

## Flow
In Django, at least three, but more commonly four, separate files are required to display one single page. Within an app, you will need to update / modify _urls.py_, _views.py_, _models.py_, and an HTML template such as _index.html_.

When a user visits your web application by typing a URL in their browser, such as https://checkseats.com/about, Django matches the URL pattern to a view.  The view determines the content for the page and collects data from a model (which is associated with a database table). The template provides the layout & styling with the end result being an HTML file sent back to the user in an HTTP response.
The flow looks something like this:
```
URL -> View -> Model (optional) -> Template
```

## Templates
Web frameworks need a convenient way to generate HTML. Django relies on templates.  Template files contains the static parts of the desired HTML output as well as some special syntax describing how other content will be inserted.

Using the code editor, create a _templates_ directory within the pages app and within that create the files called _base.html_, _home.html_, and _about.html_. In other words, your html templates should be at pages/templates/. By default, Django's template loader will look in the _templates_ folder to find a matching template.  We could modify the loader settings but not on this project.

```
└── pages
    ├── templates
        ├── base.html
        ├── home.html
        ├── about.html
```

Our base.html will be used to put code that will be reused by the other two html pages.  

\begin{codelisting}
\label{code:base_html}
\codecaption{pages/templates/base.html}
```html
<!-- pages/templates/base.html -->
<header>

  <a href="{% url 'home' %}">Home</a> | <a href="{% url 'about' %}">About</a>

</header>

{% block content %}
{% endblock content %}
```
\end{codelisting}

Our home.html page will _extend_ the base.html page.  This saves use from having to repeat certain code that will be on each page.

\begin{codelisting}
\label{code:home_html}
\codecaption{pages/templates/home.html}
```html
<!-- pages/templates/home.html -->
{% extends 'base.html' %}

{% block content %}
<h1>Homepage</h1>
{% endblock content %}
```
\end{codelisting}

Likewise, our about.html page will _extend_ the base.html page also.

\begin{codelisting}
\label{code:about_html}
\codecaption{pages/templates/about.html}
```html
<!-- pages/templates/about.html -->
{% extends 'base.html' %}

{% block content %}
<h1>About page</h1>
{% endblock content %}
```
\end{codelisting}

## URL Routing
Django uses _urls.py_ files to match URLs and route traffic. We need to modify two files.  First is the urls.py file at the project level.

\begin{codelisting}
\label{code:django_urls}
\codecaption{django\_pages/urls.py}
```python
# django_pages/urls.py
from django.contrib import admin
from django.urls import path, include # new

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('pages.urls')), # new
]
```
\end{codelisting}

After telling the project level _urls.py_ file to inlcude the pages.urls, we need to modify the _pages/urls.py_ file.

\begin{codelisting}
\label{code:django_urls}
\codecaption{pages/urls.py}
```python
# pages/urls.py
from django.urls import path
from .views import HomePageView, AboutPageView

urlpatterns = [
    path('about/', AboutPageView.as_view(), name='about'),
    path('', HomePageView.as_view(), name='home'),
]
```
\end{codelisting}

## Views

\begin{codelisting}
\label{code:pages_views}
\codecaption{pages/views.py}
```python
# pages/views.py
from django.views.generic import TemplateView

class HomePageView(TemplateView):
    template_name = 'home.html'

class AboutPageView(TemplateView):
    template_name = 'about.html'
```
\end{codelisting}

If everything is working correctly, you should be able to navigate between these two pages.

![Home](images/pages_home.png)

![About](images/pages_about.png)

## Tests
It is a very good habit to add tests to your Django app.  Tests automate the process of making sure everything is working.  This is especially important when working with other developers such that new features do not break other working code.

\begin{codelisting}
\label{code:pages_tests}
\codecaption{pages/tests.py}
```python
# pages/tests.py
from django.test import SimpleTestCase

class PageTests(SimpleTestCase):

    def test_home_page_status_code(self):
        response = self.client.get('/')
        self.assertEqual(response.status_code, 200)

    def test_about_page_status_code(self):
        response = self.client.get('/about/')
        self.assertEqual(response.status_code, 200)
```
\end{codelisting}

```
(django_pages)$ python manage.py test
System check identified no issues (0 silenced).
..
----------------------------------------------------------------------
Ran 2 tests in 0.012s

OK
```

## Git & GitHub
It is also a good idea to use Git for maintaining source code and GitHub for a remote code repository.

```
(django_pages)$ git init
(django_pages)$ git add .
(django_pages)$ git commit -m "initial commit"
```

At GitHub, create a new repository named _django_pages_ (or whatever name you want) and then push to your new remote repository. Make sure to use your GitHub username instead of _cavalryjim_.
```
(django_pages)$ git remote add origin https://github.com/cavalryjim/django_pages.git
(django_pages)$ git branch -M main
(django_pages)$ git push -u origin main
```

## Celebrate
Good job!
