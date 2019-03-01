---
title: Python知识点之内存泄露
date: 2019-03-01 11:03:58
tags: 
- Python
- Python知识点
- 内存
categories:  Python
---

## 目录

```
1. 内存泄漏是什么？
2. 内存泄露的原因
3. 内存泄露的诊断思路
4. 内存泄露的解决方法
```

![flame graph](https://raw.githubusercontent.com/AboutSange/img/master/flamegraph.png)

<!-- more -->

## 正文

### 内存泄漏是什么？

```
内存泄露是指由于疏忽或错误造成程序未能释放已经不再使用的内存。
表现形式为内存持续增长。
```

### 内存泄露的原因

```
python使用垃圾自动回收机制（引用计数、标记-清除算法、分代算法）来管理内存；那什么情况下还会产生内存泄露呢？
1.对象被另一个生命周期特别长的对象所引用；
2.循环引用中的对象定义了__del__函数；
3.第三方C模块不严谨，有内存泄露
```

### 内存泄露的诊断思路

```
1.火焰图（展示函数的执行时间）
2.memory_profiler（用来分析每行代码的内存使用情况）
3.gc（可以主动回收内存）, objgraph（可以打印对象的信息及引用，还可以打印出函数调用关系）
4.sys.getrefcount(obj) 获取对象的引用计数
```

### 内存泄露的解决方法

```
1.弱引用（weakref）
2.如果是Django发现有内存泄露的迹象，确认Debug=False即可（如为True则会保存SQL备份）
```

## 参考文章

1. [Python内存泄露调试指导思想 - JackyWu’s Blog](https://jackywu.github.io/articles/python%E5%86%85%E5%AD%98%E6%B3%84%E9%9C%B2%E8%B0%83%E8%AF%95%E6%8C%87%E5%AF%BC%E6%80%9D%E6%83%B3/)
2. [使用gc、objgraph干掉python内存泄露与循环引用！ - xybaby - 博客园](https://www.cnblogs.com/xybaby/p/7491656.html)
