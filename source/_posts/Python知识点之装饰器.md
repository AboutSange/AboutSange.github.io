---
title: Python知识点之装饰器
date: 2019-02-27 15:30:48
tags: 
- Python
- Python知识点
categories:  Python
---

## 目录

```
1. 装饰器是什么？
2. 为什么Python要引入装饰器？
3. 装饰器有利于解决哪些问题？
4. 装饰器有什么知识点？
    4.1 @是什么？
    4.2 带参数的装饰器实现
    4.3 用类实现装饰器
    4.4 多个装饰器执行顺序
    4.5 functools.wraps有什么用？
5. 装饰器的原理是什么？
```

![Decorator](https://raw.githubusercontent.com/AboutSange/img/master/decorator.jpg)

<!-- more -->

## 正文

### 装饰器是什么？

```
（1）在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）；
（2）本质上是一个返回函数的高阶函数（闭包）；
```

### 为什么Python要引入装饰器？

```
（1）便于开发，便于代码复用；
（2）它可以让其他函数在不需要做任何代码变动的前提下增加额外功能；
```

### 装饰器有利于解决哪些问题？

```
有利于解决有切面需求的场景，比如：插入日志、性能测试、事务处理、缓存、权限校验等场景
```

### 装饰器常见问题

#### @是什么？

```
@是语法糖，用以精简代码
```

#### 带参数的装饰器实现

```python
import logging
def use_logging(level):
    def decorator(func):
        def wrapper(*args, **kw):
            if level == 'warn':
                logging.warn('%s is running' % func.__name__)
            return func(*args, **kw)
        return wrapper
    return decorator

@use_logging(level="warn")
def foo(name="foo"):
    print 'i am %s' % name
    
foo()
```
装饰器等效于 
```python
foo = use_logging(level="warn")(foo)
foo()
```
运行结果如下：
```
WARNING:root:foo is running
i am foo
```

#### 用类实现装饰器

> 主要是依靠类内部的\_\_call\_\_方法

```python

class Foo(object):
    def __init__(self, func):
        self._func = func
        
    def __call__(self, *args, **kw):
        print 'class decorator running'
        self._func(*args, **kw)
        print 'class decorator ending'

@Foo
def bar(x):
    print 'bar {0}'.format(x)
    
bar(1)
```
装饰器等效于 
```python
bar = Foo(bar)
bar(1)
```
运行结果如下：
```
class decorator running
bar 1
class decorator ending
```

#### 多个装饰器执行顺序

```python
def decorator_a(func):
    print 'Get in decorator_a'
    def inner_a(*args, **kwargs):
        print 'Get in inner_a'
        return func(*args, **kwargs)
    return inner_a

def decorator_b(func):
    print 'Get in decorator_b'
    def inner_b(*args, **kwargs):
        print 'Get in inner_b'
        return func(*args, **kwargs)
    return inner_b

@decorator_b
@decorator_a
def f(x):
    print 'Get in f'
    return x * 2

f(1)
```
装饰器等效于 
```python
f = decorator_b(decorator_a(f))
f(1)
```
运行结果如下（定义时执行顺序为由内到外，调用时执行顺序为由外到内）：
```
Get in decorator_a
Get in decorator_b
Get in inner_b
Get in inner_a
Get in f
```

#### functools.wraps有什么用？

> **functools.wraps能把被调用函数的元信息（\_\_module\_\_, \_\_name\_\_, \_\_qualname\_\_, \_\_doc\_\_, \_\_annotations\_\_）赋值给装饰器返回的函数对象**

```python
import functools
def make_bold(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        return '<b>{0}</b>'.format(func(*args, **kwargs))
    return wrapper
```

### 装饰器的原理是什么？

```
装饰器本质是一个返回函数的高阶函数。
装饰器放在一个函数头上相当于将这个函数对象当成参数传给装饰函数去执行，在定义时就执行（没调用也会执行）
```

## 参考文章

1. [如何理解Python装饰器？ - python教程的回答 - 知乎](https://www.zhihu.com/question/26930016/answer/360300235)
2. [装饰器-廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386819879946007bbf6ad052463ab18034f0254bf355000)
3. [如何理解Python装饰器？ - 刘志军的回答 - 知乎](https://www.zhihu.com/question/26930016/answer/99243411)
4. [Python 装饰器执行顺序迷思 - Python提高班 - SegmentFault 思否](https://segmentfault.com/a/1190000007837364)
5. [简单地理解Python的装饰器 —— 0xFEE1C001](https://www.lightxue.com/understand-python-decorator-the-easy-way)