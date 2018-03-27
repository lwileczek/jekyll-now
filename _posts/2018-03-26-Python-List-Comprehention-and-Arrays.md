# Iterating through a list
Python neophytes often make the common mistake of using ```range(len(x))``` when looping through an object. For example, 
```python
new_list = []
for i in range(len(x)):
  y = i + 1
  new_list.append(y)
```
The code above accomplishes the goal of creating a new list where each element is one larger than in the prevous list.  However, by the beautiful magic powers of Python, we are able to accomplish this faster and in less lines.
We are able to use the ```timeit``` package from python to see how fast our code runs. I used python 3.6.4 for the following code.
First we must come up with a problem statement. Let's say we have a list of numbers and we want to know what the percent change is from the element previous.
Now we need to set some variables that we'll want to reuse during this test. 
```python
from timeit import timeit
my_setup = """
import numpy as np
x = np.arange(1000))
"""
n = 10000
```
Now let's dive into the speed tests.
We'll start with the Niave approve.
```python
print(timeit("""
lst = []
for i in range(1,len(x)):
    percent = (x[i-1] - x[i])/x[i]
    lst.append(percent)
""",
setup = my_setup, number = n)
)
```
The code above ran in approximately **4.5 seconds** on my machine.  Our first change will be cutting the code down from four lines to one.  I mean, come on, typing is so exhausting!
Do do this we'll use what is called [list comprehension](https://docs.python.org/3.6/tutorial/datastructures.html#list-comprehensions).
Basically, we can do the *for loop* inside of brackets and it'll generate the list we want. 
```python
print(timeit("""
[(x[i-1] - x[i])/x[i] for i in range(1,len(x))]
""",
setup = my_setup, number = n)
)
```
This code range in about **4.0 seconds**. Just about 10% faster than the naive approach and much cleaner. 
Now the goal is to stop using ```range(len(x))```. Python allows us to pluck out each element of an iterable object and use that
instead of just numbers. for example you could just write ```for i in list``` which will iterate using each element of the iterable so you don't have to continuously refer
back to the object (e.g. with 'list[i]' you can just say 'i'). A quick example is:
```python
lst = ["Python", "is", "great!"]
for i in lst:
  print(i)
```
Which will print:
```
Python
is
great!
```
Many objects in python are iterable like lists, dictionaries, arrays, and Pandas DataFrames.
Now let's implement this into our code sample.
```python
print(timeit("""
[(new-old)/old for old,new in zip(x[:-1], x[1:])]
""",
setup = my_setup, number = n)
)
```
This code runs in about **2.6 seconds**. Excelsior! we improved our code by about 33% by simple removing range(len(x)) and using list comprehension. 
When possible, we should avoid looping at all. This is especially prudent when we are dealing with very large objects. This could be an array that is 1,000,000 long or a dataframe with 250,000 rows.
Instead, we should perform what are called [vectorized](https://en.wikipedia.org/wiki/Array_programming) operations where we act on the entire array at once. 
```python
print(timeit("""
(x[:-1]-x[1:])/x[1:]
""",
setup = my_setup, number = n)
)
```
This code runs in about **0.05 seconds**. That's correct, almost 2 orders of magnatude faster the the original code. As the size of the object grows, the difference between the two method grows. 
A warning though! When objects are *small*, list comprehensions may be the better choice. It's always best to speed test your code and find out what works for your situation.
