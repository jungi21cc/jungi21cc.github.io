---
layout: post
title: Python numpy
tags: [Computer Science]
---

1. *Numpy operations*

- comparison

```python
a = np.array([1, 2, 3, 4])
b = np.array([4, 2, 2, 4])
a == b
# array([False,  True, False,  True])

a > b
# array([False, False,  True, False])
```

- Extrema

```python
x = np.array([1, 3, 2])
x.min()
#1
x.max()
#3

x.argmin()  # index of minimum
#0

x.argmax()  # index of maximum
#1
```

- broadcasting

```python
a = np.tile(np.arange(0, 40, 10), (3, 1)).T
a
# array([[ 0,  0,  0],
#        [10, 10, 10],
#        [20, 20, 20],
#        [30, 30, 30]])

b = np.array([0, 1, 2])
a + b
# array([[ 0,  1,  2],
#        [10, 11, 12],
#        [20, 21, 22],
#        [30, 31, 32]])
```

```python
a = np.arange(0, 40, 10)
a.shape
# (4,)

a = a[:, np.newaxis]  # adds a new axis -> 2D array
a.shape
# (4, 1)
a
# array([[ 0],
#        [10],
#        [20],
#        [30]])array([[
b = np.array([0, 1, 2])
a + b
# array([[ 0,  1,  2],
#        [10, 11, 12],
#        [20, 21, 22],
#        [30, 31, 32]])
```

2. *Numpy Array shape manipulation*

- flatten

```python
a = np.array([[1, 2, 3], [4, 5, 6]])
a.flatten()
# array([1, 2, 3, 4, 5, 6])
a.T
# array([[1, 4],
#        [2, 5],
#        [3, 6]])
a.T.flatten()
# array([1, 4, 2, 5, 3, 6])
```

- reshape

```python
a.shape
#(2, 3)(2, 3)
a.reshape(6,1)
# array([[1],
#        [2],
#        [3],
#        [4],
#        [5],
#        [6]])
```

- adding dimension

```python
z = np.array([1, 2, 3])
z
#array([1, 2, 3])
z[:, np.newaxis]
# array([[1],
#        [2],
#        [3]])
z[np.newaxis, :]
#array([[1, 2, 3]])
```

3. *Numpy sort data*

```python
a = np.array([[4, 3, 5], [1, 2, 1]])
a.sort(axis=1)
a
# array([[3, 4, 5],
#        [1, 1, 2]])

```

```python
a = np.array([4, 3, 1, 2])
j = np.argsort(a)
j
# array([2, 3, 1, 0])
a[j]
# array([1, 2, 3, 4])
```

```python
a = np.array([4, 3, 1, 2])
j_max = np.argmax(a)
j_min = np.argmin(a)
j_max, j_min
# (0, 2)
```


***
*Reference*

[basic operations](http://www.scipy-lectures.org/intro/numpy/operations.html)
