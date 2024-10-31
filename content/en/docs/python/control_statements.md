---
title: Control Statements
# date: 2017-01-05
description: >
  A short lead description about this content page. It can be **bold** or _italic_ and can be split over multiple paragraphs.
# categories: [Examples]
# tags: [test, sample, docs]
weight: 130
---

# Control Statements

> I've got my own mind.
> Wanna make my own decisions.
> When it has to do with my life.
> I wanna be the one in **control**.
> —Janet Jackson, *Control*

![Controlling Puppet](images/puppet.jpg)

## Conditional Statements
It is very common for programs to execute statements based on some conditions. In this section we will learn about Python's **if**, **else**, and **elif** statements.

\begin{aside}
\label{aside:relational_operators}
\heading{Relational operators}

\noindent Relational operators are used to compare values. It either returns **True** or **False** according to the condition.

| Operator |	Meaning	| Example |
|---------|---------|---------|
| >	| Greater than - True if left operand is greater than the right |	x > y |
| <	| Less than - True if left operand is less than the right |	x < y |
| == |	Equal to - True if both operands are equal |	x == y |
| != |	Not equal to - True if operands are not equal	| x != y |
| >= |	Greater than or equal to - True if left operand is greater than or equal to the right |	x >= y |
| <= |	Less than or equal to - True if left operand is less than or equal to the right |	x <= y |
|=======

\end{aside}

### The *if* statement
The **if** statement evaluates a boolean condition.  If the results of the evaluation is **True**, the code block following the **if** statement is executed.  If the evaluation is **False**, the code block is not executed.

\begin{codelisting}
\label{code:basic_if}
\codecaption{control.py}
```python
today = "Tuesday"

if today == "Tuesday":
  print("We get to write code!")
```
\end{codelisting}

Create a file using the code above and run the command ```python control.py``` in your terminal.
```
$ python control.py
We get to write code!

```

### Adding *else*
The **else** statement only works when following an **if** statement and the block code following the **else** gets executed only when the **if** statement evaluates to **False**.

\begin{codelisting}
\label{code:basic_if_else}
\codecaption{control.py (modified)}
```python
today = "Wednesday"

if today == "Tuesday":
  print("We get to write code!")
else:
  print("Time to learn Scrum!")
```
\end{codelisting}

Running the modified file, should provide this output:
```
$ python control.py
Time to learn Scrum!
```

### The *elif* statement
Similar to the **else** statement, an **elif** statement only works when following an **if** statement but the **elif** allows you to evaluate another boolean condition.  If the **elif** statement evaluates to **True** the code block following will be executed.

\begin{aside}
\label{aside:membership_operators}
\heading{Membership operators}

\noindent Keywords **in** and **not in** are membership operators in Python. They are used to test whether a value or variable is found in a container (string, list, or dictionary).  In a dictionary we can only test for presence of key, not the value.

| Operator |	Meaning	| Example |
|---------|---------|---------|
| in	| True if value/variable is found in the sequence |	5 in x |
| not in |	True if value/variable is not found in the sequence |	5 not in x |
|=======

\end{aside}

\begin{codelisting}
\label{code:basic_elif}
\codecaption{control.py (modified)}
```python
today = "Saturday"
code_days = ["Tuesday", "Thursday"]
scrum_days = ["Monday", "Wednesday", "Friday"]

if today in code_days:
  print("We get to write code!")
elif today in scrum_days:
  print("Time to learn Scrum!")
else:
  print("Time to relax!")
```
\end{codelisting}

Make the above changes to the control.py file and run it in the terminal.
```
$ python control.py
Time to relax!
```

Let's use the **if-elif-else** statement combination to help with assigning grades.

\begin{codelisting}
\label{code:letter_grade}
\codecaption{letter\_grade.py (modified)}
```python
num = int(input("Enter your numeric grade: "))
if num >= 90:
  letter_grade = 'A'
elif num >= 80:
  letter_grade = 'B'
elif num >= 70:
  letter_grade = 'C'
elif num >= 60:
  letter_grade = 'D'
else:
  letter_grade = 'F'

print("Your letter grade is " + letter_grade )
```
\end{codelisting}

## For Loops
Thus far, we have limited ourselves to short sequences of instructions that are executed one after the other.  Next we will introduce repetition statements which repeat an action.  Each repetition of the action is known as a **pass** or an **iteration**.  

The Python **for** loop is a control statement that supports a definite number of iterations.

\begin{codelisting}
\label{code:for_loop}
\codecaption{for\_loop.py }
```python
weekdays = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"]

for day in weekdays:
  print(day)
```
\end{codelisting}

**for** loops are also great for iterating over numbers and making calculations.

\begin{codelisting}
\label{code:avg_loop}
\codecaption{avg\_loop.py }
```python
l = [5, 6, 10, 20]

sum = 0
for num in l:
    sum += num

avg = sum / len(l)
print(avg)
```
\end{codelisting}

## While Loops
The **for** loop executes for a definite number of iterations.  In some situations, the number of iterations needed is not predictable.  The logic needed requires a condition to be met before completing its work.

\begin{codelisting}
\label{code:blackjack}
\codecaption{blackjack.py }
```python
import random

card_values = [2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10, 11]
sum = 0
turn = "hit"
while turn == "hit":
  sum += random.choice(card_values)
  print("You currently have: " + str(sum))
  turn = input("What do you want to do? ")

if sum >= 21:
  print("You went over 21!")
else:
  print("You stopped at: " + str(sum))
```
\end{codelisting}

As a final note on **while** loops, the command **break** can also be used to exit a loop.

## Nested Loops
Loops can be nested in that you can put any type of loop inside of any other type of loop. For example a for loop can be inside a while loop or vice versa.  Let's expand the the simple Blackjack code such that it allows us to play multiple games.

\begin{codelisting}
\label{code:blackjack}
\codecaption{blackjack.py (modified) }
```python
import random

card_values = [2, 3, 4, 5, 6, 7, 8, 9, 10, 10, 10, 10, 11]
play_again = 'yes'
while play_again == 'yes':
  sum = 0
  turn = "hit"
  while turn == "hit":
    sum += random.choice(card_values)
    print("You currently have: " + str(sum))
    turn = input("What do you want to do? ")

  if sum >= 21:
    print("You went over 21!")
  else:
    print("You stopped at: " + str(sum))

  play_again = input("Want to play again? ")
```
\end{codelisting}
