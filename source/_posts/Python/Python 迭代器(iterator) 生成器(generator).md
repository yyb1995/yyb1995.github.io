---
title: Python 迭代器(iterator) 生成器(generator)
categories: Python
tags: Python
icon: note
---

## 可迭代对象(iterable)

- 定义

   iterable又称为可迭代对象。在Python中，如果一个对象（又称为container object）实现了`__iter__()`方法或者可以支持下标索引的`__getitem__()`方法，就称为一个可迭代对象。

- `__iter__()`方法

   调用一个可迭代对象的`__iter__()`方法会返回一个当前可迭代对象的一个迭代器(iterator)。这个在下一章会提到。

- `iter()`方法

   调用一个可迭代对象的`iter()`方法会返回一个当前可迭代对象的一个迭代器(iterator)。

- 判断方法
  在Python中，我们常用到的集合数据类型都是iterable的。如列表(list)，元组(tuple)，字典(dict)，集合(set)，字符串(str)等。在程序中判断一个对象是否是iterable的方法为
  
  ```python
  # Method1: use isinstance
  from collections import Iterable
  isinstance([1, 2, 3], Iterable)

  # Method2: use for loop
  for i in [1, 2, 3]:
      print(i)
  ```

## 迭代器(iterator)

- 定义

   借用[Python文档](https://docs.python.org/dev/howto/functional.html#iterators)中对迭代器的定义，一个迭代器指的是一个包含着数据流的对象，它每次返回数据中的一个元素。一个可迭代对象本身也可以是迭代器，但是这样在迭代一次之后数据本身将无法使用，除非将迭代器重置。在Python中大部分迭代器和可迭代对象是分离的，这样就保证了能够重复产生迭代器。

- `__next__()`方法
   
   一个迭代器必须实现`__next__()`方法。在每次调用**迭代器**（注意，不是可迭代对象）的`__next__()`方法时顺序返回数据的一个元素。当遍历完数据后抛出`StopIteration`异常，表示迭代完成。这时迭代器也就完成了它的使命。

- `__iter__()`方法

   调用一个迭代器的`__iter__()`方法会返回迭代器对象本身，注意每次调用产生一个新的迭代器对象。

- `next()`方法

   在`next()`内传入一个迭代器，可以得到迭代器的下一个值。


## 生成器(generator)

- 定义

   生成器可以看成是迭代器的简化版。生成器只需要定义一个函数，并将普通函数的`return`改为`yield`即可。


- 使用
   在使用生成器时，可以像一个普通的迭代器一样，使用`for in `或者`next()`方法获取其中的值。
   
   ```python
   # Define a generator
   def range_like(n):
       i = 0
       while i < n:
           yield i
           i += 1
   
   # Create a generator object
   a = range_like(5)
   a.__next__()

   for i in range_like(n):
       print(i)
   ```

## 迭代器、生成器的关系
迭代器、生成器、可迭代对象的关系可用下图表示
![](https://i.loli.net/2017/05/06/590d99e937114.png)

## for语句的执行顺序

在对一个iterable对象使用`for`语句时，实际上先调用了`iter()`方法返回一个iterator，然后再依次调用`__next__()`方法获得每一个值。当抛出`StopIteration`异常时意味着`for`语句的结束。


## 参考

1. [What exactly are iterator, iterable and iteration](https://stackoverflow.com/questions/9884132/what-exactly-are-iterator-iterable-and-iteration)

2. [9.8. Iterators](https://docs.python.org/3/tutorial/classes.html#iterators)

3. [彻底理解Iterable, Iterator, generator](https://www.jianshu.com/p/24876cf14a5c)

4. [Python中iteration(迭代)、iterator(迭代器)、generator(生成器)等相关概念的理解](https://blog.csdn.net/dawningblue/article/details/72629362)