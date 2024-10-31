---
title: Files
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

![Files](images/files.png)

Thus far, most of our input & output has been limited to the command line.  In many situations, we will need to work with larger sets of data.  Python easily allows for reading from and writing to files.

The **open()** method returns a file object, and is most commonly used with two arguments: **open(filename, mode)**.

The first argument is a string containing the filename. The second argument is another string containing a few characters describing the way in which the file will be used. *mode* can be ```'r'``` when the file will only be read, ```'w'``` for only writing, and ```'a'``` opens the file for appending; any data written to the file is automatically added to the end. ```'r+'``` opens the file for both reading and writing.

## Reading from a file
As mentioned above to read a file, use the **open(filename, 'r')** method.  For reading a file, the mode argument is optional; ```'r'``` will be assumed if it’s omitted.

\begin{codelisting}
\label{code:read_file}
\codecaption{read\_file.py}
```python
file = open("text_file.txt")

for line in file:
    print(line)

file.close()
```
\end{codelisting}

## Writing to a file
When opening a file using **open(filename, 'w')**, the returned file object can be used to write  information to the file but any existing files with the same name will be erased when this mode is activated.

\begin{codelisting}
\label{code:write_file}
\codecaption{write\_file.py}
```python
students = [ "Jennifer", "Craig", "Robert", "Boudreaux", "Thibodeaux"]
file = open("output_file.txt", "w")
file.write("Here is a list of names: \n")

for s in students:
    file.write(s + "\n")

file.close()
```
\end{codelisting}

## Appending a file
Similar to writing to a file, you can also append new text to the already existing file instead of overwriting an existing file.

\begin{codelisting}
\label{code:append_file}
\codecaption{append\_file.py}
```python
more_students = ["Sonja", "Melissa", "Karen"]
file = open("output_file.txt", "a")
file.write("Here are some more names:\n")

for s in more_students:
    file.write(s + "\n")

file.close()
```
\end{codelisting}

## The *with* statement
When finished working with a file, use the **close()** method to end the interaction with the file. This terminates the file object and makes the file available to other resources.

While it is a good programming habit to close files after use, we, as busy human beings, can sometimes forget to close a file after opening it.  

Luckily, Python has a **with** keyword that helps us better manage file resources. It is good practice to use **with** when using file objects. The advantage is that the file is properly closed after its code block has completed.

\begin{codelisting}
\label{code:with_statement}
\codecaption{with\_open.py}
```python
with open("text_file.txt") as file:
    for line in file:
        print(line)

```
\end{codelisting}

## JSON
With files, the **read()** and **write()** methods only use strings.  This can be problematic when trying to use files to store or share data that isn't easily converted to and from a string.  

Rather than having users constantly writing and debugging code to save complicated data types to files, Python allows you to use the popular data interchange format called **JSON (JavaScript Object Notation)**. The standard module called ```json``` can take Python data hierarchies, and convert them to string representations; this process is called serializing. Reconstructing the data from the string representation is called deserializing.

\begin{codelisting}
\label{code:json_output}
\codecaption{create\_json.py}
```python
import json

student_scores = {'name': 'John', 'scores': [90, 88, 92]}
with open("student_scores.json", "w") as file:
    json.dump(student_scores, file)
```
\end{codelisting}

\begin{codelisting}
\label{code:json_input}
\codecaption{read\_json.py}
```python
import json

with open("student_scores.json", "r") as file:
    student_scores = json.load(file)

print("The object type is: ", type(student_scores))
print(student_scores)
```
\end{codelisting}

This simple serialization technique can handle lists and dictionaries, but serializing arbitrary class instances in JSON requires a bit of extra effort. The Python documents reference for the [json](https://docs.python.org/3/library/json.html#module-json) module contains an explanation of this.

\begin{aside}
\label{aside:json_api}
\heading{Language of APIs}

\noindent JSON is the language of devices communicating over the Internet.  Roughly 80% of all existing APIs use JSON and it is very likely that over 95% of all new APIs are using JSON.  Anyone looking for a career in technology or data should be familiar with the producing and consuming JSON.

Here is an example of a JSON feed from an OpenData portal: [Baton Rouge Crime](https://data.brla.gov/resource/fabb-cnnu.json)

\end{aside}

## Other file types
Data can be shared using multiple other file types, such as **.csv**, **.xls**, and **.xlsx** files.  To properly read these specifically formatted files, Python usually requires additional packages that can easily be added with the **import** command.  

\begin{codelisting}
\label{code:read_csv}
\codecaption{mpg.py}
```python
import csv

with open('mpg.csv') as file:
    mpg_reader = csv.reader(file)
    for row in mpg_reader:
        print(row)
```
\end{codelisting}
