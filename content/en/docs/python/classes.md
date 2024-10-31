---
title: Python Class
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

# Classes
In science, we use classifications to help describe the objects we are studying.  Zoologist use terms such as mammals, birds, fish, reptiles, amphibians in the description of animals.  This is the **class** of the animal.  Knowing an animals class will provide a basic understanding of that species.

![Animal classification](images/animal_classification.jpg)

In *object oriented programming*, classes provide a means of bundling data and functionality together.  An *object* is a software entity that contains data and the ability to execute functions.  The data contained in an object is known as the object's *data attributes*.  Those attributes are simply variables that reference data.  The functions that an object can call are known as *methods*.  

## Data & code

![An object](images/object_data.png)

Utilizing objects helps to separate code from the data.  Before object oriented programming, code was written in a more procedural format with variables and other data structures, such as lists or dictionaries.  Changes at an application's data-layer could cause multiple errors depending on the application size.  With objects, changes in the database are handled inside the object's class thus minimizing the effort required to accommodate changes.

## Object reusability
In addition to solving problems of code and data separation, object oriented programming empowers developers to reuse objects.  An object is not meant to be a stand-alone program but it can be reused in multiple other programs.

## Defining the object
![House blueprint](images/house_blueprint.png)
Objects are defined a class.  A class is code that specifies the data attributes and methods of a particular type of object.  Think of a class as a "blueprint" from which an object is created.  In construction, a blueprint is used to describe a structure, such as a house, that will be built.  The blueprint itself is not a house object, but a description of a house.  When the blueprint is used to build an actual house, it can be called an *instance* of the house described by the blueprint.

Perhaps an even simpler example would be a cookie cutter and a cookie.  The cookie cutter would be the class that is used to create the cookies.

\begin{codelisting}
\label{code:dog_class}
\codecaption{class\_dog.py}
```python
class Dog:

    # the __init__ method initializes the object.
    def __init__(self, name):
        self.name = name
        self.tricks = []    # creates a new empty list for each dog

    def add_trick(self, trick):
        self.tricks.append(trick)
```
\end{codelisting}

To view the class, we will use the REPL / interactive Python.  Inside a terminal, type 'python'.

```python
>>> from class_dog import Dog
>>> d = Dog('Eli')
>>> type(d)
<class 'class_dog.Dog'>
>>> d.name
'Eli'
>>> d.tricks
[]
>>> d.add_trick("roll over")
>>> d.add_trick("fetch ball")
>>> d.tricks
['roll over', 'fetch ball']
```

So, a class is a description of an object's characteristics.  When used in a program, a class can be used to create one or more objects.  Each object created from a class is called an *instance* of the class.

## Class definitions
To create a class, we write a *class definition*.  A class definition is a set of statements that define a class's methods and data attributes.

Suppose you are writing an application to simulate a Magic 8 Ball.  In the application, we need to repeatedly 'shake' the Magic 8 Ball and each time return an answer.  Let's see what that class might look like.

\begin{codelisting}
\label{code:magic_ball_class}
\codecaption{class\_magic8ball.py}
```python
import random

class Magic8Ball:
    answers = ["It is certain.", "It is decidedly so.", "Without a doubt.",
               "Yes - definitely.", "You may rely on it.", "As I see it, yes.",
               "Most likely.", "Outlook good.", "Signs point to yes.",
               "Yes.", "Reply hazy, try again", "Ask again later.",
               "Better not tell you now.", "Cannot predict now.",
               "Concentrate and ask again.", "Cannot predict now.",
               "Concentrate and ask again.", "Don't count on it.",
               "My reply is no.", "My sources say no.",
               "Outlook not so good.", "Very doubtful."]

    # the __init__ method initializes the object.
    def __init__(self):
        self.answer = ""

    def shake(self):
        self.answer = random.choice(self.answers)

    def get_answer(self):
        return self.answer
```
\end{codelisting}

Get ready for some Magic 8 Ball action and use the interactive Python in your terminal.

```python
>>> from class_magic8ball import Magic8Ball
>>> ball = Magic8Ball()
>>> type(ball)
<class 'class_magic8ball.Magic8Ball'>
>>> ball.get_answer()
''
>>> ball.shake()
>>> ball.get_answer()
'Yes - definitely.'
```

## Class inheritance
Classes can inherit from other classes. A class can inherit attributes and methods from another class, called the *superclass*. A class which inherits from a superclass is called a *subclass*, also called child class. Superclasses are sometimes called ancestors as well. There exists a hierarchy relationship between classes. It's similar to relationships or categorizations that we know from real life.

\begin{codelisting}
\label{code:class_inheritance}
\codecaption{class\_inheritance.py}
```python
class Person:

    def __init__(self, first, last):
        self.firstname = first
        self.lastname = last

    def full_name(self):
        return self.firstname + " " + self.lastname

class Employee(Person):

    def __init__(self, first, last, employee_id):
        Person.__init__(self, first, last)
        self.employee_id = employee_id

    def employee_name(self):
        return self.full_name() + ", " +  str(self.employee_id)
```
\end{codelisting}

Once again, open the terminal and type 'python'.

```python
>>> from class_inheritance import Person, Employee
>>> type(e)
<class 'class_inheritance.Employee'>
>>> e = Employee("james", "davis", 87)
>>> e.employee_name()
'james davis, 87'
```

We have overridden the method \__init\__ from Person in Employee.  Method overriding is an object-oriented programming feature that allows a subclass to provide a different implementation of a method that is already defined by its superclass or by one of its superclasses. The implementation in the subclass overrides the implementation of the superclass by providing a method with the same name, same parameters or signature, and same return type as the method of the parent class.

## Object Attributes

In object-oriented programming (OOP), objects are created from classes.  Python comes with several built-in attributes for all objects.  Documentation for these attributes can be located here:
[docs.python.org](https://docs.python.org/3/library/stdtypes.html#special-attributes).

Of particular interest and usefulness, is the ```object.__dict__``` attribute.  Using ```__dict__``` on any object returns a dictionary store of object’s (writable) attributes.

```python
>>> from class_dog import Dog
>>> d = Dog('Eli', 'Golden')
>>> d.__dict__
{'name': 'Eli', 'breed': 'Golden', 'tricks': ['Eat', 'Sleep']}
```

The ```__dict__``` attribute works very well with the *json* package for creating json files.

```python
>>> import json
>>> json.dumps(d.__dict__)
'{"name": "Eli", "breed": "Golden", "tricks": ["Eat", "Sleep"]}'
```
