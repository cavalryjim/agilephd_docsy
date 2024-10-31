---
title: Stripe
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---

![stripe](images/stripe.jpg)

Stripe is a great resource for entrepreneurs to quickly start collecting payments from customers.  In general, we (small business) do not want to store our customer's credit card or bank information.  Doing so could expose you financial liabilities and PR nightmares if you have a data breach.  If you do this long enough, data loses are a "when" issue and not an "if".

Luckily, Stripe will handle this for us!  Stripe will allow you to conduct one time charges or reoccurring charges depending on your business model.  Stripe provides plugins for all of the popular web and mobile development frameworks to include Django.

A version of this code can be viewed on [GitHub](https://github.com/cavalryjim/tweeter_v7).

If you haven't already started your virtual environment, start it.
```
$ pipenv shell
```

## Install Stripe
The good folks at Stripe have created a python package that makes interacting with their API much easier.  Use pipenv to install the package.

```
$ pipenv install stripe
```

## Setup a Stripe account
You will need a [Stripe account](https://stripe.com/). Once you have an account, navigate to the [dashboard](https://dashboard.stripe.com/dashboard).  In the "Developers" section, you will need the API keys.

Stripe accounts come with two API key pairs: a secret / publishable pair for testing and a secret / publishable pair for production.  Safeguard your secret key.  Eventually, you will want to research using environmental variables.

Toggle the "test data" switch in the righthand nav bar.  For this demo, we will use the test keys.

## Payment app
Let's create a payment app that will contain the logic that will be used to integrate with Stripe.

```
$ python manage.py startapp payments
```

Add the payment app to the tweeter_app/settings.py file.

\begin{codelisting}
\label{code:add_payments}
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
    'django.contrib.sites',

    'rest_framework',
    'rest_framework.authtoken',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'rest_auth',
    'rest_auth.registration',
    'bootstrap4',
    'bootstrap_datepicker_plus',

    'users',
    'tweets',
    'api',
    'payments', # new
]

...
```
\end{codelisting}

Update the tweeter_app/urls.py file.

\begin{codelisting}
\label{code:urls}
\codecaption{tweeter\_app/urls.py}
```python
# tweeter_app/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('tweets.urls')),
    path('users/', include('users.urls')),
    path('api/v1/', include('api.urls')),
    path('api-auth/', include('rest_framework.urls')),
    path('api/v1/rest-auth/', include('rest_auth.urls')),
    path('api/v1/rest-auth/registration/', include('rest_auth.registration.urls')),
    path('payments/', include('payments.urls')), # new
]
```
\end{codelisting}

## Process a Stripe Payment
Inside the Payments app, we will update the urls with a paths for capturing a credit card and creating a charge.

Create a payments.urls.py file with the following code.

\begin{codelisting}
\label{code:urls_payments}
\codecaption{payments/urls.py}
```python
# payments/urls.py
from django.urls import path

from . import views

urlpatterns = [
    path('', views.PaymentPageView.as_view(), name='payment'),
    path('charge/', views.charge, name='charge'),
]
```
\end{codelisting}

For the view logic, we will use a generic template view for the payment page.  The payment html page will also use a Stripe JavaScript link such that we are not directly handling credit card numbers.

Charging a card is usually a two step process.  The first step authorizes the card (valid, sufficient funds, etc.) and the second step creates the charge.  Our second step will use the function 'charge'.  See the following code.

As a note, you will need to research the use of environmental variables to better safeguard your Stripe secret key...especially when you move to production.  For now, just know that anyone that has access to your github repository can also see your Stripe keys.

\begin{codelisting}
\label{code:views_payments}
\codecaption{payments/views.py}
```python
# payments/views.py
from django.views.generic.base import TemplateView
from django.shortcuts import render
import stripe

stripe.api_key = 'sk_test_*********************' # use your secret key

class PaymentPageView(TemplateView):
    template_name = 'payment.html'

def charge(request):
    if request.method == 'POST':
        charge = stripe.Charge.create(
            amount=500,  # amount is in cents.
            currency='usd',
            description='A Django charge',
            source=request.POST['stripeToken']
        )
        return render(request, 'charge.html')
```
\end{codelisting}

The payments.html template uses a stripe js link such that we don't need to directly interact with a customer's credit card.  Charge amounts are in cents such that we are not dealing with decimals.  Additionally, you are putting your Stripe publishable key in the data-key section of the form.  

\begin{codelisting}
\label{code:html_payments}
\codecaption{payments/templates/payment.html}
```html
<!-- payments/templates/payment.html -->
{% extends 'base.html' %}
{% block title %}Tweeter | Payment{% endblock %}

{% block content %}
  <h2>Checkout</h2>
  <form action="{% url 'charge' %}" method="post">
    {% csrf_token %}
    <script src="https://checkout.stripe.com/checkout.js" class="stripe-button"
            data-key="pk_test_*******************"
            data-description="A Django Charge"
            data-amount="500"
            data-locale="auto">
    </script>
  </form>
{% endblock content %}
```
\end{codelisting}

This is the html page users will see when the charge has been successfully completed.

\begin{codelisting}
\label{code:html_charge}
\codecaption{payments/templates/charge.html}
```html
<!-- payments/templates/charge.html -->
{% extends 'base.html' %}
{% block title %}Tweeter | Payment{% endblock %}

{% block content %}
  <h2>Thanks for your payment!</h2>
{% endblock content %}
```
\end{codelisting}

## Testing
Using the Stripe testing secret / publishable key pair, you can use the test credit card number 4242 4242 4242 4242.  Any future date and 3 digit pin will complete the credit card information.

![payment page](images/payment_page.png)

Looking at the test data on the Stripe dashboard, you should see any charges you just created.
![stripe charge](images/stripe_charge.png)
