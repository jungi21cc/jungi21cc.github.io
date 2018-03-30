---
layout: post
title: Python Intermediate
tags: [Computer Science, Python]
---

**list comprehension**
>


```python
>>> squares = []
>>> for x in range(10):
...     squares.append(x**2)
...
>>> squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

can express list comprehension:
<br/>

```python
squares = list(map(lambda x: x**2, range(10)))
```
or, equivalently:
<br/>

```python
squares = [x**2 for x in range(10)]
```




- *map*
>

- *reduce*
>



- *filter*




**generator** and **iterator'**
>


```python
1 # Build and return a list
2 def firstn(n):
3     num, nums = 0, []
4     while num < n:
5         nums.append(num)
6         num += 1
7     return nums
8
9 sum_of_first_n = sum(firstn(1000000))
```
useing generator

```python
   1 # Using the generator pattern (an iterable)
   2 class firstn(object):
   3     def __init__(self, n):
   4         self.n = n
   5         self.num, self.nums = 0, []
   6
   7     def __iter__(self):
   8         return self
   9
  10     # Python 3 compatibility
  11     def __next__(self):
  12         return self.next()
  13
  14     def next(self):
  15         if self.num < self.n:
  16             cur, self.num = self.num, self.num+1
  17             return cur
  18         else:
  19             raise StopIteration()
  20
  21 sum_of_first_n = sum(firstn(1000000))
```

rewrite the above iterator as a generator function:
<br/>

```python
1 # a generator that yields items instead of returning a list
2 def firstn(n):
3     num = 0
4     while num < n:
5         yield num
6         num += 1
7
8 sum_of_first_n = sum(firstn(1000000))
```




**args** and **kwargs**


***
*Reference*
