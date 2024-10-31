---
title: Sprint Zero
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
date: 2017-01-05
weight: 4
---


In agile software development frameworks, such as Scrum, development is conducted in time-boxed work iterations called Sprints.  Sprint Zero is all the work that needs to happen before you can start actually development[^sprint_zero_footnote].

## Python versions
Technology is constantly changing.  That the great thing about our industry and the frustrating thing about our industry.  In Python, there was a bit of a riff when the language went from version 2.x to 3.x.  The debate is mostly over but be aware that some popular packages were slow to embrace Python 3 and developers were forced to choose between versions.

![Python versions](images/python-2-vs-python-3.jpg)

Despite a few staunch holdouts, Python 3 is the present and future of the language.  You can read more about the version debate at [wiki.python.org](https://wiki.python.org/moin/Python2orPython3).

For this book, we're going to use Python 3.  If you're using MacOS or Linux, you probably have a version of Python already installed.  Verify by typing 'python --version' in the terminal.
```
$ python --version
Python 3.6.3
```

If you have Python version 3.6 or later, you are good and can skip the section on installing Python.

## Installing Python
There are multiple ways to install Python 3 on your computer. While possible, I would advise against downloading it directly from the official [Python website](https://www.python.org/) as that approach can cause issues with PATH variables, updates, and uninstalls.

### MacOS Installation
In the past, MacOS came with Python 2 already installed.  This is great except you really do not want to start a new project using an outdated version of Python.  

I recommend using [Homebrew](https://brew.sh/) for installing Python3 on a Mac.  Homebrew is a popular package manager that  installs the stuff you need that Apple didn’t.

The first step for installing Homebrew is to install XCode.  XCode is Apple's development environment and you will use it if you ever get into iOS development.
```
$ xcode-select --install
```

Next, paste the following command into the terminal to install Homebrew.
```
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Confirm Homebrew is installed by running the following. (You may also see warnings that can likely be ignored.)
```
$ brew doctor
Your system is ready to brew.
```

You can now use Homebrew to install Python3 (and multiple other packages).
```
$ brew install python3
```

Confirm your python version.
```
$ python3 --version
Python 3.7.5
```

### Windows Installation
Because 1) we are a community and 2) I am less familiar with installing Python on a Windows computer, I am going to _punt_ and provide some external sources for installing on Windows.

The first is [https://installpython3.com/windows/](https://installpython3.com/windows/). These instructions are for Windows 10 and seem to be current and well curated.  If you are not using Windows version 10, my next suggestion would be to use the Anaconda distribution of Python [https://www.anaconda.com/distribution](https://www.anaconda.com/distribution).  The Anaconda distribution is geared more for data science but it comes with a convenient Anaconda prompt and does not interfere with other PATH settings.

## Virtual Environments
Virtual environments are isolated containers containing the software dependencies for a given project. This is important because by default Python packages, such as Django, are installed in the same directory. This is problematic because if you do this long enough, you will eventually work on projects that use different versions of Python packages. You may join an existing project built using Django 1.11 but all your current projects are using Django 2.2.  It would be very difficult to juggle upgrading / downgrading packages. With virtual environments, the community has solved the problem for you.

Python developers will debate certain development approaches but we all use virtual environments.  __Use a dedicated virtual environment for each new Python project.__

For this book, we will use [Pipenv](https://pipenv.kennethreitz.org/en/latest/).  To install Pipenv we can use pip, a Python package manager, which should have been included with your Python installation.

```
$ pip install pipenv
```

## Install Django
To ensure everything is installed and working, we will create a test Django project.  Using Pipenv, we will install Django in a new project folder called _django_test_.  I literally use a main project folder called _Projects_.  You can put your project wherever you wish.

```
$ cd Projects
$ mkdir django_test
$ cd django_test
```
Once you have created and navigated into your _django_test_ directory, install the Django package using Pipenv.

```
$ pipenv install django
```

Activate the virtual environment with the command 'pipenv shell'.

```
$ pipenv shell
Spawning environment shell (/bin/bash). Use 'exit' to leave.
source /Users/james/.local/share/virtualenvs/django_test-LKSBG718/bin/activate
bash-3.2$ source /Users/james/.local/share/virtualenvs/django_test-LKSBG718/bin/activate
(django_test) $
```

Create the django_test project using the 'django-admin startproject django_test' command.

```
(django_test) $ django-admin startproject django_test .
```

This is the file structure created for you.

```
└── django_test
    ├── manage.py
    └── django_test
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py
```

Start the Django development server.

```
(django_test) $ python manage.py runserver
```

If it is working correctly, you should be able to see a success message in your browser [localhost:8000](http://localhost:8000).

![Success](images/install_successfully.png)

Once you are finished with a virtual environment session, you can deactivate it by using the command 'exit'.

```
(django_test) $ exit
exit
MacBook-Pro-6:django_test james$
```

Do not worry if it is a little confusing right now. The basic pattern is to install new packages with _pipenv_, activate them with _pipenv shell_, and then _exit_ when done.

## Celebrate
You are complete with Sprint Zero and ready to move forward!

[^sprint_zero_footnote]: Some agile practitioners argue there is no such thing as Sprint Zero.  My response to them would be - this is my book and I'm going to use the term as I see fit.
