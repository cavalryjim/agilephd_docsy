---
title: Python Containers
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

Containers are built-in Python data structures that allow other data types to be organized, assessed, and manipulated.  In this chapter, we explore the most commonly used data structures: the **list** and the **dictionary**.

![Containers](images/cargo_ship.png)

Lists and dictionaries provide powerful ways to organize data in useful and interesting applications.  In addition to exploring the use of lists and dictionaries, this chapter also introduces some simple control statements.

## Lists
A **list** is a sequence of data values called items or elements.  An item can be of any type.

A Python list is similar to a list you would make in the real-world:

- shopping list
- to-do list
- roster for a team
- guest list for a party

### List structure
The logical structure of a list resembles the structure of a string.  Items in a list are ordered by position.  Each list item has a unique index specifying its position.  Like many programming languages, Python is 0-index based, meaning list indexes start at 0, not 1.  The index for a list starts with 0 and counts upward.

A list is written as a bracketed sequence of data separated by commas.  Here are some examples:

[1971, 1989, 1994]                # A list of integers  
['bananas', 'apples', 'oranges']  # A list of strings  
[[4, 5], [200, 343]]              # A list containing two other lists  

When using variables, a list is defined by either using the bracket notation **[]** or by casting any iterable sequence with the **list()** function.  

Start an interactive Python session by using the ```python``` command in a terminal.

```python
>>> a = [1, 2, 3, 4]
>>> a
[1, 2, 3, 4]
>>> type(a)
<class 'list'>
>>> b = list("Hello world!")
>>> b
['H', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd', '!']
>>> type(b)
<class 'list'>
```

For convenience, we will be using the **range()** function to assist with list creation.  The range function takes one to three arguments.

- list(range(4))        # create a list up to, but not including, the integer 4.
- list(range(2, 6))     # create a list of integers starting at 2 and going to, but not including, 6.
- list(range(3, 18, 3)) # create a list starting at 3, going to 18, and using steps of 3.

```python
>>> r = list(range(3, 18, 3))
>>> r
[3, 6, 9, 12, 15]
>>> type(r)
<class 'list'>
>>> len(r)
5
>>> 12 in r
True
>>> 13 in r
False
```

### Modifying lists
At any point in a list's existence, elements can be inserted, removed, or changed.  The list will maintain its identity but the contents can change.

```python
>>> t = [34, 12, 24, 77, 234, 65]
>>> type(t)
<class 'list'>
>>> t[1]
12
>>> t[1] = 9
>>> t
[34, 9, 24, 77, 234, 65]
```

Note that the subscript operation ```t[1]``` refers to the element's position and the target of the assignment.  

Use the **split** function to extract a list of words from a sentence.

```python
>>> s = "This is a sentence with seven words."
>>> words = s.split()
>>> words
['This', 'is', 'a', 'sentence', 'with', 'seven', 'words.']
```

The **list** object includes several methods for inserting and removing elements.

| List Method | Results |
|---------|---------|
| list.append(element)   | Adds element to the end of the list  |
| list.extend(a_list)   | Adds another list to the list   |
| list.insert(index, element)   | Inserts element at index  |
| list.pop()   | Removes and returns the element at the end of the list   |
| list.pop(index) | Removes and returns the element at index |
|=======

The **append** method takes the new element as an argument and adds it the the end of the list.  The method **insert** does something similar but it takes the index and new element as arguments and adds the element to the given position by shifting the remaining elements to the right.  The **extend** method takes another list and adds those elements to the end of the list.

```python
>>> e = [1, 2, 3, 4, 5, 6]
>>> e.append(7)
>>> e
[1, 2, 3, 4, 5, 6, 7]
>>> e.extend([8, 9, 10])
>>> e
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> e.insert(5, 5.5)
>>> e
[1, 2, 3, 4, 5, 5.5, 6, 7, 8, 9, 10]
```

The **pop** method is used to remove an element from a list.  If the index position is not specified, **pop** removes and returns the last element of the list.  If a position is specified, **pop** removes the element at that location and returns it.  Remaining elements would then be shifted one position to the left.

```python
>>> e
[1, 2, 3, 4, 5, 5.5, 6, 7, 8, 9, 10]
>>> e
[1, 2, 3, 4, 5, 5.5, 6, 7, 8, 9, 10]
>>> e.pop()
10
>>> e.pop(4)
5
>>> e
[1, 2, 3, 4, 5.5, 6, 7, 8, 9]
```

### Sorting
The **list** object has a **sort** method that will arrange its elements in numeric or alphabetical order.  

```python
>>> f = [23, 12, 500, 3.3, 42, 92, 7]
>>> f
[23, 12, 500, 3.3, 42, 92, 7]
>>> f.sort()
>>> f
[3.3, 7, 12, 23, 42, 92, 500]
```

### Aliasing
Not all variable names refer to different variables. When two identifiers refer to the same variable (and therefore value), this is known as an **alias**.

```python
>>> first = [20, 31, 42]
>>> second = first
>>> first
[20, 31, 42]
>>> second
[20, 31, 42]
>>> second.append(53)
>>> second
[20, 31, 42, 53]
>>> first
[20, 31, 42, 53]
```

In the example above, a single list object is created with two names, or aliases.  When an element is appended to the **second** list, the **first** list changed also.  This happens because the variables **first** and **second** refer to the exact same list object.

![List Aliases](images/list_alias.png)

If you do not want an alias, you can pass the source list to a call of the **list** function.

```python
>>> third = list(first)
>>> third
[20, 31, 42, 53]
```

![List Copy](images/list_copy.png)

### Equality
Frequently, programmers need to check for equality between variables (are the values equal).  There could also be circumstances when you need to know not only if the values are equal but do the variables refer to the same object.

The **==** operator returns **True** if two values are equal, or the lists have the same structural equivalence.  The Python **is** operator returns **True** if two variables refer to the same object.

```python
>>> first == second
True
>>> first == third
True
>>> first is second
True
>>> first is third
False
```

## Dictionaries
A **dictionary** is a collection which is unordered, changeable and indexed.

![Containers](images/dictionary-clipart.png)

### Dictionary structure
A dictionary, or dict, organizes data values by association with other data values rather than by sequential position.  A dictionary is a collection which is unordered, changeable and indexed. In Python dictionaries are written with curly brackets, and they have keys and values.  Dictionaries are initialized using curly braces **{ }**.

```python
>>> capitals = {'United States': 'Washington, DC','France': 'Paris','Italy': 'Rome'}
>>> capitals
{'United States': 'Washington, DC', 'France': 'Paris', 'Italy': 'Rome'}
>>> type(capitals)
<class 'dict'>
>>> capitals['Italy']
'Rome'
>>> capitals['Spain'] = 'Madrid'
>>> capitals
{'United States': 'Washington, DC', 'France': 'Paris', 'Italy': 'Rome',
'Spain': 'Madrid'}
>>> 'Germany' in capitals
False
>>> 'Italy' in capitals
True
>>> morecapitals = {'Germany': 'Berlin','United Kingdom': 'London'}
>>> capitals.update(morecapitals)
>>> capitals
{'United States': 'Washington, DC', 'France': 'Paris', 'Italy': 'Rome',
'Spain': 'Madrid', 'Germany': 'Berlin', 'United Kingdom': 'London'}
>>> len(capitals)
6
```

### Dictionary methods
Python has a set of built-in methods that you can use on dictionaries.

| Dictionary Method |	Results |
|---------|---------|
| dict.copy()	| Returns a copy of the dictionary |
| dict.fromkeys() | Returns a dictionary with the specified keys and values |
| dict.get() | Returns the value of the specified key |
| dict.items() | Returns a tuple for each key value pair |
| dict.keys() | Returns the dictionary's keys |
| dict.pop() | Removes the element with the specified key |
| dict.popitem() | Removes the last key-value pair |
| dict.setdefault() | Returns the value of the specified key. If the key does not exist: insert the key, with the specified value |
| dict.update() | Updates the dictionary with the specified key-value pairs |
| dict.values() | Returns a list of all the values in the dictionary |
|=======

Let's try some of these functions.

```python
>>> capitals.items()
dict_items([('United States', 'Washington, DC'), ('France', 'Paris'),
('Italy', 'Rome'), ('Spain', 'Madrid'), ('Germany', 'Berlin'),
('United Kingdom', 'London')])
>>> capitals.keys()
dict_keys(['United States', 'France', 'Italy', 'Spain', 'Germany',
'United Kingdom'])
>>> capitals.values()
dict_values(['Washington, DC', 'Paris', 'Rome', 'Madrid', 'Berlin', 'London'])
>>>
```

### Loop through a Dictionary
As an iterable object, we can loop, or iterate, through a dictionary using a **for** loop.  With the **for** loop we can execute a set of statements, once for each item in a list, tuple, set etc.

```python
>>> for key in capitals.keys():
...     print(key)
...
United States
France
Italy
Spain
Germany
United Kingdom

>>> for value in capitals.values():
...     print(value)
...
Washington, DC
Paris
Rome
Madrid
Berlin
London

>>> for key, value in capitals.items():
...     print(key, value)
...
United States Washington, DC
France Paris
Italy Rome
Spain Madrid
Germany Berlin
United Kingdom London
```

\begin{aside}
\label{aside:reading_files}
\heading{Reading from a file}

\noindent In Python you need to give access to a file by opening it. You can do it by using
the open() function. Open returns a file object, which has methods and attributes
for getting information about and manipulating the opened file.

Using the **with** statement, you get better syntax and exceptions handling.  In addition, it will automatically close the file. The with statement provides
a way for ensuring that a clean-up is always used.

```python
>>> file = open("welcome.txt")
>>> data = file.read()
>>> print(data)
Hi, this is a text file!

>>> file.close()  # It's important to close the file when you're done with it
```

Opening a file using with is as simple as: ```with open(filename) as file```:

```python
>>> with open("welcome.txt") as file: # Use file to refer to the file object
...     data = file.read()
...     print(data)

```
\end{aside}
