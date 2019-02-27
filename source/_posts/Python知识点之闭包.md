---
title: Python知识点之闭包
date: 2019-02-27 18:29:17
tags: 
- Python
- Python知识点
categories:  Python
---

## 目录

```
1. 作用域（LEGB）
2. 闭包是什么？
3. 为什么要用闭包？
4. 为什么通过闭包能实现在函数之外访问函数的局部变量？
```

![closure](https://raw.githubusercontent.com/AboutSange/img/master/closure.jpg)

<!-- more -->

## 正文

### 作用域（LEGB）

```
LEGB 代表名字查找顺序: locals -> enclosing function -> globals -> __builtins__
    locals 是函数内的名字空间，包括局部变量和形参
    enclosing 外部嵌套函数的名字空间（闭包中常见）
    globals 全局变量，函数定义所在模块的名字空间
    builtins 内置模块的名字空间
```

### 闭包是什么？

```
闭包（Closure），又称函数闭包，是引用了自由变量的函数。
这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。
所以，有另一种说法认为闭包是由函数和与其相关的引用环境组合而成的实体。
```

### 为什么要用闭包？

```
（1）闭包避免使用全局变量
（2）闭包允许将函数与其所操作的某些数据（环境）关联起来
（3）装饰器场景
```

### 为什么通过闭包能实现在函数之外访问函数的局部变量？


```python
def adder(x):
    def wrapper(y):
        return x + y
    return wrapper

adder5 = adder(5)
adder5(10)  # 输出 15
adder5(6)  # 输出 11
```
> 所有函数都有一个 __closure__属性，如果这个函数是一个闭包的话，那么它返回的是一个由 cell 对象 组成的元组对象。cell 对象的cell_contents 属性就是闭包中的自由变量。

```
>>> adder.__closure__
>>> adder5.__closure__
(<cell at 0x103075910: int object at 0x7fd251604518>,)
>>> adder5.__closure__[0].cell_contents
5
```

> 这解释了为什么局部变量脱离函数之后，还可以在函数之外被访问的原因的，因为它存储在了闭包的 cell_contents中了。

## 参考文章

1. [一步一步教你认识Python闭包 - FooFish-Python之禅](https://foofish.net/python-closure.html)
