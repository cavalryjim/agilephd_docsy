---
title: Functions
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

Most programs perform tasks that can be broken down into several subtasks.  For this reason, programmers usually break down programs into small manageable pieces known as functions.  A *function* is a group of statements, known as a code block, that exist within a program for the purpose of performing a specific task.  Instead of writing a large unstructured sequence of commands, it should be written as several smaller functions with each one performing a specific part of the overall task.

![Functions](images/function-machine-picture.png)

Also, functions are a key way to define interfaces so programmers can reuse or share their code.

## Built-in Functions
Python has several built-in functions that you have already been using.

```python
>>> len('Hello World!')
12
>>> type(2.0)
<class 'float'>
>>> round(3.14)
3
>>> round(3.14, 1)
3.1
```

## Writing Functions in Python
As we have seen on previous tutorials, Python makes use of blocks.

A block is a area of code of written in the format of:

```python
def block_head():
    first line of code
    second line of code
    ...
```

\begin{aside}
\label{aside:naming_conventions}
\heading{naming\_conventions}
\noindent Most programming languages have a naming convention that is used for creating the names of variables and functions.  Two popular conventions are *snake case* and *camel case*.  **Snake case**, also called *underscore case*, uses an underscore '\_' when naming a function that contains multiple words.  An example would be 'compute_pay()' or 'complete_order()'.

The **camel case** naming convention uses capitalization of the first letter of following words.  Using the same examples from above would be 'computePay()' or 'completeOrder()'.

The Python community predominately uses *snake_case*.
\end{aside}

## Void Functions
A *void function* executes a code block and then terminates.  It does not return anything.

\begin{codelisting}
\label{code:print_function}
\codecaption{function\_print.py}
```python
# Define the function
def message():
  print('I like coding.')
  print('Python is my favorite language!')

# Call the function
message()
```
\end{codelisting}

The program above only has one function, but it is common to define multiple functions in a program.  There is usually a *main* function that is called when the program starts.  The main fuction would then call other functions.

\begin{codelisting}
\label{code:two_function}
\codecaption{function\_two.py}
```python
# This program has two functions.
# First, define the main() function.
def main():
  print('I have a message for you.')
  message()
  print('See you later!')

# Define the function
def message():
  print('I like coding.')
  print('Python is my favorite language!')

# Call the function
main()
```
\end{codelisting}

## Value-returning Functions
As the name sounds, *value-returning functions* return a value when called.  The returned value can be any of the variable types we've covered (int, str, float, dict, list) or an object.

Let's write an actual function for finding the maximum value from a list of numbers.  For this example, we will use the height, in inches, for my family.

![Max height](images/max_function.png)

\begin{codelisting}
\label{code:max_function}
\codecaption{function\_max.py}
```python
def main():
  fam = [40, 55, 62, 72, 65]
  tallest = tallest(fam)
  print("The tallest is ", tallest)

def tallest(l):
  m = l[0]
  for h in l:
    if h > m:
      m = h
  return m

main()
```
\end{codelisting}

Functions can accept input, execute code, return values, or call other functions.

## Passing Arguments to Functions
Sometimes it is useful not only to call a function, but also to send some data to the function.  Additional information sent to the function is called *arguments*.

\begin{codelisting}
\label{code:multiple_arguments}
\codecaption{function\_multi\_arguments.py}
```python
# This program demonstrates a function that accepts two arguments.
def main():
    input1 = int(input('Gimme a number. '))
    input2 = int(input('Gimme another number. '))
    print('The sum of ' + str(input1) + ' and ' + str(input2) + ' is: ')
    results = calculate_sum(input1, input2)
    print(results)

# The calculate_sum function accepts two arguments.
def calculate_sum(x, y):
    return x + y

main()
```
\end{codelisting}
