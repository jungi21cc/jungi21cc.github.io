---
layout: post
title: Python Intermediate
tags: [Computer Science]
---

1. **List Comprehension**

- *list comprehension*

```python
multiples = []

for n in range(1,11):
    multiples.append(n*5)

print(multiples)
# [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]
```

```python
# list comprehension
multiples = [n*5 for n in range(1,11)]

print(multiples)
# [5, 10, 15, 20, 25, 30, 35, 40, 45, 50]
```
- *list comprehension conditional statement*

```python
evens = []

for n in range(1,11):
    if n%2 == 0:
        evens.append(n)

print(evens)
# [2, 4, 6, 8, 10]
```

```python
# list comprehension conditional statement
evens = [n for n in range(1,11) if n%2 == 0]

print(evens)
# [2, 4, 6, 8, 10]
```

```python
squares_cubes = []

for n in range(1,11):
    if n%2 == 0:
        squares_cubes.append(n**2)
    else:
        squares_cubes.append(n)

print(squares_cubes)
# [1, 4, 3, 16, 5, 36, 7, 64, 9, 100]
```

```python
# list comprehension conditional statement
squares_cubes = [n**2 if n%2 == 0 else n for n in range(1,11)]

print(squares_cubes)
# [1, 4, 3, 16, 5, 36, 7, 64, 9, 100]
```

2. **Lambda Function**

- *lambda function*

```python
def sum_func(x, y):
    return x + y

sum_func(5,6)
# 11
```

```python
# lambda function
sum_func2 = lambda x, y : x + y
sum_func2(5,6)
# 11
```

- *map*

```python
def sum_two(numberList):
    result = []
    for number in numberList:
        result.append(number + 2)
    return result
```

```python
numberList = range(1,10)
print("numberList :", list(numberList))
sum_two(numberList)
# numberList : [1, 2, 3, 4, 5, 6, 7, 8, 9]
# [3, 4, 5, 6, 7, 8, 9, 10, 11]
```


```python
# map
def sum_two_map(number):
    return number + 2
```

```python
numberList = range(1,10)
print("numberList :", list(numberList))
list(map(sum_two_map, numberList))
# numberList : [1, 2, 3, 4, 5, 6, 7, 8, 9]
# [3, 4, 5, 6, 7, 8, 9, 10, 11]
```

- map with lambda function

```python
# map with lambda function
numberList = range(1,10)
print("numberList :", list(numberList))
list(map(lambda number : number + 2, numberList))
# numberList : [1, 2, 3, 4, 5, 6, 7, 8, 9]
# [3, 4, 5, 6, 7, 8, 9, 10, 11]
````

- *filter*

```python
def get_odd(number_List):
    result = []
    for number in number_List:
        if number%2 == 0:
            result.append(number)
    return result

number_List = range(10)
get_odd(number_List)
# [0, 2, 4, 6, 8]
```

```python
# filter
def odd1(number):
    if number%2 == 0:
        return True
    return False

def odd2(number):
    return True if number%2 == 0 else False
    #or return number % 2 ==0

numberList = range(10)
list(filter(odd1, numberList)), list(filter(odd2, numberList))
# ([0, 2, 4, 6, 8], [0, 2, 4, 6, 8])
```

- filter with lambda function

```python
# filter with labmda function
numberList = range(10)
list(filter(lambda number : number % 2 == 0, numberList))
# [0, 2, 4, 6, 8]
```

- *reduce*

```python
from functools import reduce

a = [3, 1, 5, 2, 4]
reduce(lambda x, y: x + y, a)
# 15
```

- reduce with lambda function

```python
# pick the largest number
a = [3, 1, 5, 2, 4]
reduce(lambda x, y: x if x > y else y, a)
# 5
```

3. **Closure**

- *inner function*

```python
def outer(a, b):
    def inner(c, d):
        return c + d
    return inner(a, b)
```

```python
print(outer(4,7))
# 11
```


4. **generator**


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


5. **args / kwargs**

- *args*

```python
def print_args(*args):
    print(args)
    print(type(args))
    print(args[4])
    print(args[5][1])

print_args(1, 2, 3, "fast", "campus", ["data", "science"])
# (1, 2, 3, 'fast', 'campus', ['data', 'science'])
# <class 'tuple'>
# campus
# science
```

```python
def avg_func(*args):
    for data in args:
        print(data)
    return sum(args) / len(args)
```

```python
a = avg_func(100, 70, 80)
print("avg : {}".format(round(a,2)))
# 100
# 70
# 80
# avg : 83.33
```

- *kwargs*

```python
def avg_func(**kwargs):
    print(kwargs)
    print(type(kwargs))
    total, count = 0, 0
    for subject, point in kwargs.items():
        print(subject, point)
        total += point
        count += 1
    return total / count
```

```python
a = avg_func(korean=100, english=70, math=80)
print("avg : {}".format(round(a,2)))
# {'korean': 100, 'english': 70, 'math': 80}
# <class 'dict'>
# korean 100
# english 70
# math 80
# avg : 83.33
```

```python
def test_func(*args, **kwargs):
    print(args)
    print(kwargs)
```


```python
test_func(1, 2, 3, "fastcampus", "datascience")
# (1, 2, 3, 'fastcampus', 'datascience')
# {}
```

```python
test_func(korean=100, english=70, math=80)
# ()
# {'korean': 100, 'english': 70, 'math': 80}
```

```python
test_func(1, 2, 3, "fastcampus", "datascience", korean=100, english=70, math=80)
# (1, 2, 3, 'fastcampus', 'datascience')
# {'korean': 100, 'english': 70, 'math': 80}
```



***

*Reference*

[List Comprehensions in Python](https://code.tutsplus.com/ko/tutorials/list-comprehensions-in-python--cms-26836)

[파이썬 - 클로저 (Closure)](http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%81%B4%EB%A1%9C%EC%A0%80-closure/)

[파이썬 - 제너레이터 (Generator)](http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%A0%9C%EB%84%88%EB%A0%88%EC%9D%B4%ED%84%B0-generator/)
