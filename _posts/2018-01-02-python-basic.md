---
layout: post
title: Python Basic
tags: [Computer Science]

---

### 1. *Class*

```python
class calculator:

    def __init__(self, num1, num2):
        self.num1 = num1
        self.num2 = num2

    def setData(self, num1, num2):
        self.num1 = num1
        self.num2 = num2

    def add(self):
        return self.num1 + self.num2

    def sub(self):
        return self.num1 - self.num2

    def mul(self):
        return self.num1 * self.num2

    def div(self):
        return self.num1 / self.num2
```

```python
c3 = calculator(5, 7)
result = c3.add()
print(result)
```

### 2. *@property*

#### - getter / setter

```python
class Test:

def __init__(self):
self.color = "red"

#setter
def set_color(self,clr):
self.color = clr

#getter
def get_color(self):
return self.color

if __name__ == '__main__':

    t = Test()
    t.set_color("blue")

    print(t.get_color())
```

#### - property

```python
class Test:

    def __init__(self):
        self.__color = "red"

    #property
    @property
    def color(self):
        return self.__color

    @color.setter
    def color(self,clr):
        self.__color = clr

if __name__ == '__main__':

    t = Test()
    t.color = "blue"

    print(t.color)
```


### 3. *@decorator*

```python
def aaa():
  code1
  func11() # perform func1(method) operation
  code2

#decorator
@aaa
def func11():
  code1
  code2
```

```python
def foo(cls):
    pass
foo = synchronized(lock)(foo)
foo = classmethod(foo)
```

```python
@classmethod
@synchronized(lock)
def foo(cls):
    pass
```


***

*Reference*

[파이썬에서 @property 에 대해 알아보자](http://hamait.tistory.com/827)
