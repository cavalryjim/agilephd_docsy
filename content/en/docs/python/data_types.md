---
title: Data Types
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

This section will introduce the common data types used in Python.  Additional information about Python data types can be found at [docs.python.org](https://docs.python.org/3/library/datatypes.html).

![Data Types](images/data_types.jpeg)

The data types to be discussed in this chapter include:

- boolean
- numeric
- string
- datetime


\begin{aside}
\label{aside:python_repl}
\heading{Interacting with Python (REPL)}

\noindent At this point, you should have a working Python 3 interpreter at hand. If you need help getting Python set up correctly, please refer to Chapter~\ref{cha:sprint_zero} to get started.

The most straightforward way to start talking to Python is in an interactive [Read-Eval-Print Loop (REPL)](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) environment. That simply means starting up an interactive shell and typing commands to it directly.

From the terminal, you can start an interactive shell by typing the command ```python``` or ```python3```.

```
$ python
Python 3.6.3 (default, Nov  2 2017, 10:31:58)
[GCC 4.2.1 Compatible Apple LLVM 8.0.0 (clang-800.0.42.1)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>

```
If you are not seeing the \>>> prompt, then you are not talking to the Python interpreter. If you are seeing the prompt, you’re up and running! Try running some simple python commands.

```python
>>> print("Hello, World.")
Hello, World.
>>> x = 2
>>> y = 6
>>> y + x
8
>>> exit()
```
As a note, use the ```exit()``` command to get out of the Python shell environment.

\end{aside}

## Boolean
Boolean values are the two constant objects ```True``` and ```False```.

They are used to represent truth values (other values can also be considered
true or false).

In numeric contexts (for example, when used as the argument to an
arithmetic operator), they behave like the integers 1 and 0, respectively.

The built-in function ```bool()``` can be used to cast any value to a Boolean,
if the value can be interpreted as a truth value.

They are written as ```True``` and ```False```, respectively.

Open an interactive Python environment using ```python``` or ```python3``` inside your terminal.
```python
>>> t = True
>>> t + t
2
>>> f = False
>>> f
False
>>> f + f
0
>>> type(t)
<class 'bool'>
>>> type(f)
<class 'bool'>
>>> bool(1)
True
```

Boolean values respond to logical operators ```and``` / ```or```
```python
>>> True and False
False
>>> True and True
True
>>> False or True
True
>>> False or False
False
```

## Numeric
Number data types store numeric values. They are **immutable** data types, means that changing the value of a number data type results in a newly allocated object.

\begin{aside}
\label{aside:long_python2}
\heading{Python 2 vs Python 3}
\noindent
As an example of the differences between Python versions, Python 2 had two integer types ```int``` and ```long```. These have been unified in Python 3, so there is now only one type, ```int```.
\end{aside}

### Integer
Integers, or ints, are positive or negative whole numbers with no decimal point.  Use ```int()``` to cast other data types as an integer, if the value can be interpreted as an integer.

```python
>>> x = 17
>>> y = 3
>>> x + y
20
>>> x - y
14
>>> x * y
51
>>> x / y
5.666666666666667
>>> x // y
5

""" You can also cast other data types as integers """
>>> z = "22"
>>> type(z)
<class 'str'>
>>> a = int(z)
>>> type(a)
<class 'int'>
```

### Float
Floating point real values, also called floats, represent real numbers and are written with a decimal point dividing the integer and fractional parts. Floats may also be in scientific notation, with e indicating the power of 10 (2.5e2 = 2.5 x 102 = 250).  Use ```float()``` to cast other data types as a float, if the value can be interpreted as an float.

```python
>>> c = 8.0
>>> d = 20.2223
>>> pi = 3.14
>>> type(c)
<class 'float'>
>>> c * pi
25.12
>>> d / c
2.5277875
>>> d // pi
6.0
>>> d / pi
6.440222929936306
>>> i = int(c)
>>> i
8
```

## String
String literals, or just strings, in python are surrounded by either single quotation marks, or double quotation marks.

**'hello'** is the same as **"hello"**.

Strings can be output to screen using the print function. For example: ```print("hello")```.

Like many other popular programming languages, strings in Python are arrays of bytes representing unicode characters. However, Python does not have a character data type, a single character is simply a string with a length of 1. Square brackets can be used to access elements of the string.

```python
>>> h = "hello world!"
>>> type(h)
<class 'str'>
>>> h[0]
'h'
>>> h[-1]
'!'
>>> len(h)
12
>>> h.upper()
'HELLO WORLD!'
>>> h.lower()
'hello world!'
```

## DateTime
All of the above data types come "built-in" with Python.  While this makes Python a very effective language, you will likely require some additional data types not included with Python, such as dates and times.

The **datetime** module supplies classes for manipulating dates and times in both simple and complex ways.

```python
>>> import datetime
>>> d = datetime.datetime.now()
>>> type(d)
<class 'datetime.datetime'>
>>> print(d)
2018-09-06 09:51:01.246077
>>> d.year
2018
>>> print(d.strftime("%m/%d/%Y"))
09/06/2018
```

## Conclusion
Using Python, you will have many data types available to use.  Some of the data types come "built-in" to the Python language while others may require importing an additional package.

\begin{aside}
\label{aside:python_command_line}
\heading{Interacting with Python (Command Line)}

\noindent Entering commands to the Python interpreter interactively is great for quick testing and exploring features or functionality.

Eventually though, as you create more complex applications, you will develop longer bodies of code that you will want to edit and run repeatedly. You clearly don’t want to re-type the code into the interpreter every time! This is where you will want to create a reusable script file.

Using whatever code editor you’ve chosen, create a script file called **hello.py** containing the following:

\begin{codelisting}
\label{code:hello_world}
\codecaption{hello.py}
```python
print('Enter your name:')
x = input()
print('Hello, ' + x)
```
\end{codelisting}

Now save the file, keeping track of the directory or folder you chose to save into.  

Start a command prompt or terminal window. If the current working directory is the same as the location in which you saved the file, you can simply specify the filename as a command-line argument to the Python interpreter: **python hello.py**

```
$ python hello.py
Enter your name:
James
Hello, James
```
\end{aside}
