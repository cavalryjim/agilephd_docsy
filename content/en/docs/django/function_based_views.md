---
title: Function-based Views
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---

![Flow Chart](images/flow_chart.jpg)

Django allows developers to create views using two approaches; function-based views, and class-based views.  Django started out with only function-based views and added class-based views as a way to inherent functionality.

## Class-based Views
Django ships with a variety of “boilerplate” class-based views with pre-configured functionality that developers use and oftentimes extend. You may see these referred to as “generic views” because they provide solutions to common requirements. The classes have documentation on Django’s project site that shows what functionality is offered, what settings are required or possible, and how to extend them.  If your goal is to create an app with relatively straight forward CRUD actions, utilizing class-based views can speed development and reduce the amount of code you will have to write.

## Function-based Views
Function-based views take a Web request and returns a Web response. This response is usually something like an HTML file (via a template), or a redirect, or a 404 error, or whatever you choose to return. The view itself contains whatever logic is necessary to return that response.  

## Refactor
Here is an example of refactoring our django_pages app to use function-based views.  

Update the pages.url path to reference the function that will be handling the home and about requests & responses.

\begin{codelisting}
\label{code:django_urls}
\codecaption{pages/urls.py}
```python
# pages/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('about/', views.about_page, name='about'),
    path('', views.home_page, name='home'),

]
```
\end{codelisting}

Refactor the pages views.py with functions instead of classes.

\begin{codelisting}
\label{code:pages_views}
\codecaption{pages/views.py}
```python
# pages/views.py
from django.shortcuts import render

def home_page(request):
    return render(request, 'home.html')

def about_page(request):
    return render(request, 'about.html')
```
\end{codelisting}

## Options are good

Which approach is better?  That all depends on your opinion and situation.  Class-based views may be faster in most situations but function-based views give the developer more control of request-response process.
