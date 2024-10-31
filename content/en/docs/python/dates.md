---
title: Dates and Times and Strings, Oh My!
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

Working with dates & times can be frustrating.  Many systems that you may encounter require various formatting for the datetime object.  Luckily, Python is really good at handling this.

![Wizard of Oz](images/oz.jpg)

[Lions and tigers and bears, oh my!](https://www.youtube.com/watch?v=-HrfbV16-FQ)

## datetime

Python has a module named **datetime** to working with date and time objects.

\begin{codelisting}
\label{code:time_now}
\codecaption{time\_now.py}
```python
import datetime

now = datetime.datetime.now()
print(type(now))
print("Current datetime = ", now)

today = datetime.datetime.now().date()
print(type(today))
print("Current date = ", today)

current_time = datetime.datetime.now().time()
print(type(current_time))
print("Current time = ", current_time)
```
\end{codelisting}

You can also create date and time objects by designating the date or time.

\begin{codelisting}
\label{code:time_create}
\codecaption{time\_create.py}
```python
import datetime

                # year, month, day
d = datetime.date(2018, 4, 13)
print(d)

                # hour, minute, second
t = datetime.time(9, 12, 22)
print(t)

                # year, month, day, hour, minute, second
dt = datetime.datetime(2018, 4, 13, 9, 12, 22)
print(dt)
```
\end{codelisting}

We can also create date objects from a Unix timestamp. A Unix timestamp is the number of seconds since January 1, 1970 at UTC. You can convert a timestamp to date using ```fromtimestamp()``` method.

\begin{codelisting}
\label{code:time_create}
\codecaption{unix\_timestamp.py}
```python
from datetime import datetime

timestamp = datetime.fromtimestamp(1326244364)
print( "Date =", timestamp )
```
\end{codelisting}


## strptime
There are times when you may receive the datetime object as a string.  No worries, Python has a robust package for turning a string into a Python datetime object.

The ```strptime()``` method takes two arguments:

- a string representing date and time
- format code for the first argument

\begin{codelisting}
\label{code:strptime}
\codecaption{strptime.py}
```python
from datetime import datetime

date_string = "21 June, 2018"
print("date_string =", date_string)

date_object = datetime.strptime(date_string, "%d %B, %Y")
print("date_object =", date_object)
```
\end{codelisting}


## strftime
Likewise, we can take the Python datetime object and turn it into a string.

\begin{codelisting}
\label{code:strftime}
\codecaption{strftime.py}
```python
from datetime import datetime

# current date and time
now = datetime.now()

t = now.strftime("%H:%M:%S")
print("time:", t)

s1 = now.strftime("%m/%d/%Y, %H:%M:%S")
# mm/dd/YY H:M:S format
print("s1:", s1)

s2 = now.strftime("%d/%b/%Y, %H:%M:%S")
# dd/MMM/YY H:M:S format
print("s2:", s2)
```
\end{codelisting}


|Directive	|Meaning	|Example|
|---------|---------|---------|
|%a	|Abbreviated weekday name.	|Sun, Mon, ...|
|%A	|Full weekday name.	|Sunday, Monday, ...|
|%w	|Weekday as a decimal number.|	0, 1, ..., 6|
|%d	|Day of the month as a zero-padded decimal.	|01, 02, ..., 31|
|%-d	|Day of the month as a number.	|1, 2, ..., 30|
|%b	|Abbreviated month name.	|Jan, Feb, ..., Dec|
|%B	|Full month name.	|January, February, ...|
|%m	|Month as a zero-padded number.	|01, 02, ..., 12|
|%-|m	Month as a number.	|1, 2, ..., 12|
|%y|	Year without century as a zero-padded number.	|00, 01, ..., 99|
|%-y|	Year without century as a number.	|0, 1, ..., 99|
|%Y	|Year with century as a number.	|2013, 2019 etc.|
|%H	|Hour (24-hour clock) as a zero-padded number.	|00, 01, ..., 23|
|%-H|	Hour (24-hour clock) as a number.	|0, 1, ..., 23|
|%I	|Hour (12-hour clock) as a zero-padded number.	|01, 02, ..., 12|
|%-I|	Hour (12-hour clock) as a number.	|1, 2, ... 12|
|%p	|Locale’s AM or PM.	|AM, PM|
|%M	|Minute as a zero-padded number.	|00, 01, ..., 59|
|%-M|	Minute as a number.	|0, 1, ..., 59|
|%S	|Second as a zero-padded number.	|00, 01, ..., 59|
|%-S|	Second as a number.	|0, 1, ..., 59|
|%f	|Microsecond as a number, zero-padded on the left.	|000000 - 999999|
|%z	|UTC offset in the form |+HHMM or -HHMM.	 |
|%Z	|Time zone name.	 | |
|%j	|Day of the year as a zero-padded number.	|001, 002, ..., 366|
|%-j|	Day of the year as a decimal number.	|1, 2, ..., 366|
|=======

## relativedelta
Find the difference between two datetime or date objects.

\begin{codelisting}
\label{code:relativedelta}
\codecaption{relativedelta.py}
```python
from datetime import datetime
from dateutil.relativedelta import relativedelta

def main():
    birth_date = input('Enter your DOB as mm/dd/YYYY: ')
    print('your age is', age(birth_date))

def age(d):
    dob = datetime.strptime(d, '%m/%d/%Y')
    today = datetime.today()
    age = relativedelta(today, dob)
    return age.years

main()
```
\end{codelisting}
