---
title: Mixins
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---

![mixer](images/mixer.jpg)

By design, class-based views are limited in purpose. Django provides a number of mixins that provide additional functionality.

A version of the code for this chapter can be obtain here: [https://github.com/cavalryjim/tweeter_v4](https://github.com/cavalryjim/tweeter_v4).

## Must Login to Tweet
When using class-based views, we can use the [*LoginRequiredMixin*](https://docs.djangoproject.com/en/3.0/topics/auth/default/#the-loginrequired-mixin) to require that a user must be authenticated.  Non-authenicated users will be redirected to the login page.  Upon successful login, Django will return the user to the restricted content.  

Let's add the LoginRequiredMixin to the _tweets/views.py_ file such that only authenticated users can create a new tweet.

Earlier, we were selecting the user that was submitting the tweet.  This is inefficient and problematic for many reasons.  Since we require users to register and authenticate, let's automatically set a tweet's user to be the authenticated user that submitted it.

\begin{codelisting}
\label{code:tweets_views}
\codecaption{tweets/views.py}
```python
# tweets/views.py
from django.views.generic import ListView
from django.views.generic.edit import CreateView
from django.urls import reverse
from django.contrib.auth.mixins import LoginRequiredMixin #new
from .models import Tweet

class TweetListView(ListView):
    model = Tweet
    template_name = 'home.html'

class TweetCreateView(LoginRequiredMixin, CreateView): # new
    model = Tweet
    template_name = 'tweet_new.html'
    fields = ['body'] # removed user

    def get_success_url(self):
        return reverse('home')

    def get_login_url(self): # new
        return reverse('login') # new

    def form_valid(self, form): # new
        form.instance.user = self.request.user # new
        return super().form_valid(form) # new
```
\end{codelisting}

Since our code is now setting the tweet's author based on the authenticated user, we also updated the 'fields' to remove the annoying user dropdown.


## Adding Messages
In web applications, it is common to provide users with one-time notification messages (also known as “flash message”) after processing a some type of user input.  This feedback aids the user experience.

We will add the [SuccessMessageMixin](https://docs.djangoproject.com/en/3.0/ref/contrib/messages/#adding-messages-in-class-based-views) to several of the class-based views in the _users/views.py_ file.

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
from django.contrib.messages.views import SuccessMessageMixin # new

class SignUpView(SuccessMessageMixin, generic.CreateView): # new
    form_class = CustomUserCreationForm
    success_url = reverse_lazy('login')
    template_name = 'registration/signup.html'
    success_message = 'Account was created successfully.' # new

class UserUpdateView(SuccessMessageMixin, generic.UpdateView): # new
    form_class = CustomUserChangeForm
    success_url = reverse_lazy('home')
    template_name = 'update.html'
    success_message = 'User profile updated.' # new

    # This keeps users from accessing the profile of other users.
    def get_queryset(self):
        user = self.request.user
        if user.is_superuser:
            return CustomUser.objects.all()
        else:
            return CustomUser.objects.filter(id=user.id)

class UserPasswordChangeView(SuccessMessageMixin, PasswordChangeView): # new
    success_url = reverse_lazy('home')
    template_name = 'change_password.html'
    success_message = 'Password changed.' # new

class UserPasswordResetView(SuccessMessageMixin, PasswordResetView): # new
    success_url = reverse_lazy('login')
    template_name = 'reset_password.html'
    success_message = 'Check your email for a reset link.' # new
```
\end{codelisting}

## Add Tests
As always, it is proper to add automated tests for new functionality.

Update the _tweets/tests.py_ file because we are using the request.user instead of submitting a user.

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
    ...

    def test_tweet_create_view(self):
        self.client.force_login(self.user) # new
        response = self.client.post(reverse('tweet_new'),
            {'body': 'New tweet'}, follow=True) # updated
        self.assertEqual(response.status_code, 200)
        self.assertContains(response, 'New tweet')
```
\end{codelisting}

Create tests for the user module.  We will test for the ability to login, logout, signup, and update a user profile.  Since change password reset password are built-in Django methods, we will just confirm the URLs are working and return the expected template.

\begin{codelisting}
\label{code:users_tests}
\codecaption{users/tests.py}
```python
# users/tests.py
from django.contrib.auth import get_user_model
from django.test import TestCase
from django.urls import reverse

class UserTests(TestCase):
    def setUp(self):
        self.user = get_user_model().objects.create_user(
            username='testuser',
            email='test@email.com',
            password='specialpwd'
        )
        self.credentials = {'username': 'testuser', 'password': 'specialpwd'}

    def test_user_login(self):
        response = self.client.post(reverse('login'), self.credentials, follow=True)
        self.assertTrue(response.context['user'].is_active)
        self.assertEqual(response.status_code, 200)

    def test_user_logout(self):
        response = self.client.post(reverse('logout'), follow=True)
        self.assertFalse(response.context['user'].is_active)
        self.assertEqual(response.status_code, 200)

    def test_user_signup(self):
        response = self.client.post(reverse('signup'),
            {'username': 'testuser2',
             'password1': 'specialpwd2',
             'password2': 'specialpwd2'
            }, follow=True)
        self.assertEqual(response.status_code, 200)
        users = get_user_model().objects.filter(username = 'testuser2')
        self.assertEqual(users.count(), 1)

    def test_user_update_profile(self):
        u = self.user
        self.client.force_login(u)
        response = self.client.post(reverse('update_user',
            kwargs={'pk': self.user.id}),
            {'username':u.username,'email':u.email,
            'first_name':'Test', 'last_name':'Name',
            'dob':'01/22/1995', 'location':'BEC'}, follow=True)
        self.assertEqual(response.status_code, 200)
        u.refresh_from_db()
        self.assertEqual(u.last_name, 'Name')

    def test_change_password_url(self):
        self.client.force_login(self.user)
        response = self.client.get(reverse('change_password'))
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, template_name='change_password.html')

    def test_reset_password_url(self):
        response = self.client.get(reverse('reset_password'))
        self.assertEqual(response.status_code, 200)
        self.assertTemplateUsed(response, template_name='reset_password.html')
```
\end{codelisting}

## Celebrate

We now have a solid MVP and can share with our beta users after pushing to heroku.

```
(tweeter_app) $ git push heroku
(tweeter_app) $ heroku open
```

![profile page](images/profile_page.png)
