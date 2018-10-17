---
layout: post
title: Python Intermediate
tags: [Computer Science]

---

### Python Intermediate

### 1. *isinstance()*

```python
cc = list()
print(isinstance(cc, list))
#True
print(isinstance(cc, tuple))
#False
```

#### 2. *assert*

```python
assert conditions
```

```python
if not conditions:
    raise AssertionError
```

```python
def remainder(number, divisor):
    return number % divisor
```

```python
assert remainder(20, 7) == 6
```

```python
assert remainder(20, 7) == 2
```
```
---------------------------------------------------------------------------
AssertionError                            Traceback (most recent call last)
<ipython-input-11-81198d9fbb63> in <module>()
----> 1 assert remainder(20, 7) == 2

AssertionError: 
```


***

*Reference*
