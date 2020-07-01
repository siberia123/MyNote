# 装饰器

> 装饰器：将函数作为参数传入到另一个函数中，通过这种手段可以达到将传入函数增强的目的。

```python
def foo():
   print('Foo!')

def bar(func):
   print('Before')
   func()
   print('End')

bar(foo)

-------
Before
Foo!
End
-------

注意如果是这样调用：bar(foo())
结果就是这样的：
Foo!
Before
Foo!
End
它会先执行foo()函数，foo()函数执行完后，才会将该函数作为参数传入然后继续执行。
```

:star:下面开始演示真正的装饰器：

```python
def bar(func):
   def inner():
      print('Before')
      func()
      print('End')
   return inner  #注意这里返回的是函数名，而不是函数return inner()，否则在把这个当作装饰器的时候会报错！

@bar
def foo():
   print('Foo!')

foo()
```

## 装饰器的实际原理

```python
def bar(func):
   def inner():
      print('Before')
      func()
      print('End')
   return inner

def foo():
   print('Foo!')

bar(foo)()
```

## 叠加多个装饰器

```python
def bar(func):
   def inner():
      print('Before')
      func()
      print('End')
   return inner

def bar2(func):
   def inner():
      print('in bar2: Before')
      func()
      print('in bar2: End')
   return inner

@bar
@bar2
def foo():
   print('Foo!')

foo()

---------
Before
in bar2: Before
Foo!
in bar2: End
End
---------
```

如何理解这种叠加多个装饰器呢？相当于将`foo()`函数一层层包裹。

## 装饰器的运用

计时器

```python
import time

def recordTime(func):
   def inner():
      s = time.time()
      func()
      e = time.time()
      print(f'{func.__name__} finished in {e - s} seconds')
   return inner

@recordTime
def sleep():
   time.sleep(2)

sleep()
-------
sleep finished in 2.000148296356201 seconds
-------
```

有参数形式的装饰器

```python
import time

def recordTime(func):
   def inner(*args):
      s = time.time()
      func(*args)
      e = time.time()
      print(f'{func.__name__} finished in {e - s} seconds')
   return inner

@recordTime
def sum(a,b):
   print(f'{a}+{b}={a+b}')

sum(12,4534)
--------
12+4534=4546
sum finished in 0.0 seconds
--------
```

