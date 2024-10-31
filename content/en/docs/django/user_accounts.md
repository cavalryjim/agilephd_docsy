---
title: User Accounts
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 2
---

![admin login](images/user.png)

Can you think of any applications that does not require authenticating users?  Me neither.  Luckily, Django comes with a robust, built-in authentication system.

**Note: Due to the order that certain javascript files were being included, it was causing an issue with a calendar plugin. To fix the issue, an updated _templates/base.html_ will be used for this chapter.**

## Log In / Out
You can add the authentication module anywhere in your project but I prefer the user app. Update the project _urls.py_ file to include the users app.

\begin{codelisting}
\label{code:tweeter_urls}
\codecaption{tweeter\_app/urls.py}
```python
# tweeter_app/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tweets.urls')),
    path('users/', include('users.urls')), #new
]
```
\end{codelisting}

Django provides a default view for login and logout features. To use it, we need to add a url pattern for the auth system and a couple templates.  Create a _users/urls.py_ file.

While we are editing _users/urls.py_, let go ahead and add paths for changing / resetting passwords, signing up as a new user, and updating the profile of an existing user.

\begin{codelisting}
\label{code:users_urls}
\codecaption{users/urls.py}
```python
# users/urls.py
from django.urls import path, include
from .views import SignUpView, UserUpdateView, UserPasswordChangeView, UserPasswordResetView # new

urlpatterns = [
    path('accounts/', include('django.contrib.auth.urls')),
    path('change_password/', UserPasswordChangeView.as_view(), name='change_password'),
    path('reset_password/', UserPasswordResetView.as_view(), name='reset_password'),
    path('signup/', SignUpView.as_view(), name='signup'),
    path('<int:pk>/update/', UserUpdateView.as_view(), name='update_user'),
]
```
\end{codelisting}

We update the _settings.py_ file telling the framework where to redirect a user after login or logout.

\begin{codelisting}
\label{code:settings}
\codecaption{tweeter\_app/settings.py}
```python
...
BOOTSTRAP4 = { 'include_jquery': True }

LOGIN_REDIRECT_URL = 'home' # new
LOGOUT_REDIRECT_URL = 'home' # new
...
```
\end{codelisting}

Create a template for the login page.

\begin{codelisting}
\label{code:login_html}
\codecaption{users/templates/registration/login.html}
```html
<!-- users/templates/registration/login.html -->
{% extends 'base.html' %}
{% load bootstrap4 %}
{% block content %}

  <h4>
    Log In
    <small><a href="{% url 'reset_password' %}">(Lost password?)</a></small>
  </h4>
  <form method="post">{% csrf_token %}
    {% bootstrap_form form %}
    {% buttons submit='Ok' %}{% endbuttons %}
  </form>

{% endblock content %}
```
\end{codelisting}

Let’s update our base.html template so we display links for logging in / out and signup. We can use the *is_authenticated* attribute for this.

\begin{codelisting}
\label{code:base_html}
\codecaption{templates/base.html}
```html
<!-- templates/base.html -->
{% load bootstrap4 %}
<html>
<head>
{% bootstrap_css %}

{% block extrahead %}
{% endblock %}
<title>Tweeter</title>
</head>

<body>

  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <a class="navbar-brand" href="{% url 'home' %}">Tweeter</a>
    <button class="navbar-toggler" type="button" data-toggle="collapse"
    data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent"
    aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        {% if user.is_authenticated %}
          <li class="nav-item">
            <a class="nav-link" href="{% url 'tweet_new' %}">New Tweet</a>
          </li>
        {% endif %}
      </ul>

      <ul class="navbar-nav ml-auto">
        {% if user.is_authenticated %}
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink"
            data-toggle="dropdown" aria-haspopup="true"
            aria-expanded="false">
              Welcome {{ user.username }}
            </a>
            <div class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
              <a class="dropdown-item" href="{% url 'update_user' user.pk %}">Profile</a>
              <a class="dropdown-item" href="{% url 'logout' %}">Log Out</a>
            </div>
          </li>
        {% else %}
        <li class="nav-item">
          <a class="nav-link" href="{% url 'signup' %}">Sign Up</a>
        </li>
          <li class="nav-item">
            <a class="nav-link" href="{% url 'login' %}">Log In</a>
          </li>
        {% endif %}
      </ul>

    </div>
  </nav>

  <div class="container">
      {% autoescape off %}{% bootstrap_messages %}{% endautoescape %}
      {% block content %}(no content){% endblock %}
  </div>

{% bootstrap_javascript jquery='full' %}
{% block extrascript %}{% endblock %}
</body>
</html>
```
\end{codelisting}


## User Registration
At the moment, the only way to create new users is through the admin module.  While that is great for users with admin privilages, we need a process for users to signup / register.


Update the _users/views.py_ file to define the SignUpView, UserUpdateView, UserPasswordChangeView, and UserPasswordResetView that we just referenced in the _urls.py_ file.

\begin{codelisting}
\label{code:users_views}
\codecaption{users/views.py}
```python
# users/views.py
from django.urls import reverse_lazy
from django.views import generic
from .forms import CustomUserCreationForm, CustomUserChangeForm
from .models import CustomUser
from django.contrib.auth.views import PasswordChangeView, PasswordResetView

class SignUpView(generic.CreateView):
    form_class = CustomUserCreationForm
    success_url = reverse_lazy('login')
    template_name = 'registration/signup.html'

class UserUpdateView(generic.UpdateView):
    form_class = CustomUserChangeForm
    success_url = reverse_lazy('home')
    template_name = 'update.html'

    # This keeps users from accessing the profile of other users.
    def get_queryset(self):
        user = self.request.user
        if user.is_superuser:
            return CustomUser.objects.all()
        else:
            return CustomUser.objects.filter(id=user.id)

class UserPasswordChangeView(PasswordChangeView):
    success_url = reverse_lazy('home')
    template_name = 'change_password.html'

class UserPasswordResetView(PasswordResetView):
    success_url = reverse_lazy('login')
    template_name = 'reset_password.html'
```
\end{codelisting}

Our SignUpView is subclassing the generic view, CreateView. We specify the use of a CustomUserCreationForm and a template at signup.html. And we use reverse_lazy to redirect the user to the log in page upon successful registration.

The UserUpdateView is subclassing another generic view, UpdateView, and specifies the use of CustomUserChangeForm & template _update.html_.  For this class, we create a function *def get_queryset* that will limit the users that can be updated.  If you are a superuser, the function returns all of the CustomUser objects.  Otherwise, the function only returns the CustomUser object for the user submitting the request (the current user).

The UserPasswordChangeView and UserPasswordResetView are subclassing built-in Django authentication classes.

## DatePicker
Because we created a CustomUser in an earlier chapter, we now need to create a CustomUserCreationForm that will be used when creating and updating user accounts.

\begin{codelisting}
\label{code:users_forms}
\codecaption{users/forms.py}
```python
# users/forms.py
from django.contrib.auth.forms import UserCreationForm, UserChangeForm
from .models import CustomUser
from django import forms
from bootstrap_datepicker_plus import DatePickerInput

class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm):
        model = CustomUser
        fields = UserCreationForm.Meta.fields + ('email', )

class CustomUserChangeForm(UserChangeForm):
    password = None

    class Meta:
        model = CustomUser
        fields = ['username','email', 'first_name', 'last_name', 'dob', 'location' ]
        widgets = { 'dob': DatePickerInput() }
```
\end{codelisting}

For the CustomUserChangeForm, this will be used with the user's edit profile page.  For the date of birth (or dob), we are using a widget that pops up a calendar.  The helps validate our user inputs an actual date (versus letting them type in a string and hope it is in the correct format 'mm/dd/YYYY').

We also need to install the datepicker package using pipenv.
```
(tweeter_app) $ pipenv install django-bootstrap-datepicker-plus
```

Update _tweeter_app/settings.py_ to add bootstrap_datepicker_plus as an installed app.

\begin{codelisting}
\label{code:settings2}
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

    'bootstrap4',
    'bootstrap_datepicker_plus', # new

    'users',
    'tweets',
]
...
```
\end{codelisting}

You can learn more about configuring the [datepicker widget here](https://github.com/monim67/django-bootstrap-datepicker-plus).

## Templates
Now, we create the corresponding templates.

This is the template used when new users signup.

\begin{codelisting}
\label{code:signup_html}
\codecaption{users/templates/registration/signup.html}
```html
<!-- users/templates/registration/signup.html -->
{% extends 'base.html' %}
{% load bootstrap4 %}
{% block content %}

  <h4>Sign Up</h4>
  <form method="post">{% csrf_token %}
    {% bootstrap_form form %}
    {% buttons submit='Ok' %}{% endbuttons %}
  </form>

{% endblock content %}
```
\end{codelisting}

This template is used for managing / updating an existing user's profile.

\begin{codelisting}
\label{code:signup_html}
\codecaption{users/templates/update.html}
```html
<!-- users/templates/update.html -->
{% extends 'base.html' %}
{% load bootstrap4 %}

{% block extrascript %}
  {{ form.media }}
{% endblock %}

{% block content %}
  <h4>
    Profile
    <small><a href="{% url 'change_password' %}">(change password)</a></small>
  </h4>
  <form method="post">{% csrf_token %}
    {% bootstrap_form form %}
    {% buttons submit='Ok' %}{% endbuttons %}
  </form>
{% endblock content %}
```
\end{codelisting}

Create a template for users to change their password.

\begin{codelisting}
\label{code:change_password_html}
\codecaption{users/templates/change\_password.html}
```html
<!-- users/templates/change_password.html -->
{% extends 'base.html' %}
{% load bootstrap4 %}
{% block content %}

  <h4>Change Password</h4>
  <form method="post">{% csrf_token %}
    {% bootstrap_form form %}
    {% buttons submit='Ok' %}{% endbuttons %}
  </form>

{% endblock content %}
```
\end{codelisting}

Create a template for users to reset their password.

\begin{codelisting}
\label{code:reset_password_html}
\codecaption{users/templates/reset\_password.html}
```html
<!-- users/templates/reset_password.html -->
{% extends 'base.html' %}
{% load bootstrap4 %}
{% block content %}

  <h4>Reset Password</h4>
  <form method="post">{% csrf_token %}
    {% bootstrap_form form %}
    {% buttons submit='Ok' %}{% endbuttons %}
  </form>

{% endblock content %}
```
\end{codelisting}

## Conclusion
Our Tweeter app is looking more like an actual deployable app!  Code for this increment can be obtained on [GitHub](https://github.com/cavalryjim/tweeter_v3).
