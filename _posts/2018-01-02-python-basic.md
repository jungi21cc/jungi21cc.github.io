---
layout: post
title: Python Basic
tags: [Computer Science]

---

### Python Basic

### 1. *Class*

```python
class calculator:
    #constructor
    def __init__(self, num1, num2):
        self.num1 = num1
        self.num2 = num2
    #destructor
    def __del__(self):
        print('call destructor')

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

del c3
#call desctuctor
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
  print(t.color)
  t.set_color("blue")
  t.get_color()
  print(t.color)
#red
#blue
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
    print(t.color)
    t.color = "blue"
    print(t.color)
#red
#blue
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

### 4. *constructor Initialization*

```Python



```

### 5. *Class variable vs Instance variable*


```Python
class Account:
  num_account = 0
  def __init__(self, name):
      self.name = name
      Account.num_accounts += 1
  def __del__(self):
      Account.num_accounts -= 1

kim = Account('Kim')
lee = Account('lee')

print(kim.name)
#kim
print(kim.name)
#lee
```


***

*Reference*

[파이썬에서 @property 에 대해 알아보자](http://hamait.tistory.com/827)
[파이썬 - OOP Part 4. 클래스 메소드와 스태틱 메소드 (Class Method and Static Method)](http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-oop-part-4-%ED%81%B4%EB%9E%98%EC%8A%A4-%EB%A9%94%EC%86%8C%EB%93%9C%EC%99%80-%EC%8A%A4%ED%83%9C%ED%8B%B1-%EB%A9%94%EC%86%8C%EB%93%9C-class-method-and-static-method/)
[클래스 변수와 인스턴스 변수](https://wikidocs.net/1744)
