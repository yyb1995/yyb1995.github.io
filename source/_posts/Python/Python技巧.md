---
title: Python常见问题和技巧
categories: Python
tags: Python
icon: note
---

## 1 模块修改后再次载入无变化
``` python
import importlib
importlib.reload(PackageName)
%load_ext autoreload
%autoreload 2
```
## 2 python数据类型set
set是一种无序集合。常见的方法有：
   1. 创建集合
`a = set('python')`
   2. 向集合中传入一个元素并拆分
`a.update('java')`
   3. 删除集合中元素
`a.remove('p')`
   4. 不同集合之间的关系
![](http://www.iplaypython.com/uploads/allimg/131215/2-131215203406215.jpg)

## 3 利用列表生成字典
```python
a = [1, 2, 3]
b = [4, 5, 6]
dict1 = dict(zip(a, b))
```

## 4 python 排序
`sorted([1, 2, 3, 5, 3])`

## 5 python中的队列
```
from collections import deque
queue = deque(['a', 'b', 'c', 'd'])
queue.append('e')
queue.popleft()
```

## 6 python数组按某列排序
`data[data[:,2].argsort()]`
数组按第三列排序
## 7 python 字典转变量
`globals().update(dict)`
例如： {foo: bar} -> foo = bar
## 8 Google注释规范
```python
def fetch_bigtable_rows(big_table, keys, other_silly_variable=None):
    """Fetches rows from a Bigtable.

    Retrieves rows pertaining to the given keys from the Table instance
    represented by big_table.  Silly things may happen if
    other_silly_variable is not None.

    Args:
        big_table: An open Bigtable Table instance.
        keys: A sequence of strings representing the key of each table row
            to fetch.
        other_silly_variable: Another optional variable, that has a much
            longer name than the other args, and which does nothing.

    Returns:
        A dict mapping keys to the corresponding table row data
        fetched. Each row is represented as a tuple of strings. For
        example:

        {'Serak': ('Rigel VII', 'Preparer'),
         'Zim': ('Irk', 'Invader'),
         'Lrrr': ('Omicron Persei 8', 'Emperor')}

        If a key from the keys argument is missing from the dictionary,
        then that row was not found in the table.

    Raises:
        IOError: An error occurred accessing the bigtable.Table object.
    """
    pass
```
**注意：** 函数功能注释与参数说明之间应该空一行

## 9 map、reduce和filter
map用于将一个函数映射到后面给出的变量列表中，reduce用于将一个函数依次映射到后面的相邻变量中，reduce用于筛选列表中符合某个函数条件的值
```python
map(lambda x: x ** 2, range(1, 10))
from functools import reduce
reduce(lambda x, y: x + y, range(1, 11))
filter(lambda x: x % 2, range(1, 11))
```

## 10 *号的使用
*号可用在函数的输入参数前，用于将输入参数解包以匹配函数的形参
```python
def print_(a, b, c):
    print(a)
    print(b)
    print(c)

a = ['ap', 'ba', 'or']
print(*a)
```

## 11 Python数组拷贝
在进行Python的数组拷贝时，有时会用到切片操作。下面是一个例子：
```python
dec_seq = torch.rand(3, 4, 5)
dec_input = dec_seq[:, :-1]
dec_output = dec_seq[:, 1:]
```
实际上dec_input和dec_output只是对dec_seq的一个引用，并不是对对象的一个完整复制。当dec_seq的值改变时，dec_input和dec_output的值也会随之改变。因此，dec_input和dec_output更像是指向dec_seq一部分的指针。如果想对原数组进行完整的拷贝，应该使用copy模块中的copy和deepcopy。
1. copy.copy 浅拷贝 只拷贝父对象，不会拷贝对象的内部的子对象。
2. copy.deepcopy 深拷贝 拷贝对象及其子对象
例子：
```python
>>> import copy
>>> a = [1,2,3,4,['a','b']]  #原始对象

>>> b = a  #赋值，传对象的引用

>>> c = copy.copy(a)

>>> d = copy.deepcopy(a)

>>> a.append(5)
>>> a[4].append('c')

>>> print 'a=',a
a= [1, 2, 3, 4, ['a', 'b', 'c'], 5]
>>> print 'b=',b
b= [1, 2, 3, 4, ['a', 'b', 'c'], 5]
>>> print 'c=',c
c= [1, 2, 3, 4, ['a', 'b', 'c']]
>>> print 'd=',d
d= [1, 2, 3, 4, ['a', 'b']]
```

## 12 Python的异常处理机制
python有两种机制可以触发异常：
1. try-except-else-finally
程序尝试执行try下的语句，如果触发异常就跳到相应的except语句，如果没有错误就跳到else语句。finally语句是无论是否发生错误都会执行。
```python
try:
<语句>        #尝试执行
except <异常类型1>：
<语句>        #如果在try部份引发了'name'异常
except <异常类型2>，<数据>:
<语句>        #如果引发了'name'异常，获得附加的数据
else:
<语句>        #正常执行
finally:
<语句>        #无论是否触发异常都执行的语句
```

2. if-raise
程序尝试判断if语句下是否成立，如果成立，抛出raise下的error。
```python
if expression:
    raise Exception
```

3. 自定义Error类型
 需要自定义异常类型时，需要继承`Exception`类
 ```python
class SomeCustomException(Exception):
    pass
 ```

4. warnings模块
warnings.warn()函数用于生成警告信息。格式为：
```python
if expression:
    warnings.warn('Warning message', WarningType)
```
其中WarningType可以是Python模块中的warning，[具体目录](https://docs.python.org/3/library/warnings.html)

## 13 Python filter()函数
`filter(function, iterable)`

filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。

该接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判，然后返回 True 或 False，最后将返回 True 的元素放到新列表中。

使用方法：
```python
filter1 = filter(lambda x: x > 0, [-1, 0, 1, 5])
filter1.next()
```

## 14 对于None的判断
在 if判断中，None，False，空字符串，0，空列表，空字典，空元组都会被判断成False。因此在使用if not x判断是否为None时，需要排除其余可能的影响。因此，建议的判断方式为：if x is not None

## 15 字符串join()方法
Python join()方法用于将序列中的元素以指定的字符连接生成一个新的字符串。str.join(sequence)即用str来连接sequence
`a = ''.join('a', 'b', 'c', 'd')`

## 16 响应键盘终止
```python
except KeyboardInterrupt:
    print('Interrupt')
```

## 17 字符串不转义
在使用正则表达式时，通常不希望表示转义字符，方法是在字符串前加上`r`，如`r'[]\'`。

## 18 找出数组中k个最大/最小值
```python
import heapq
list1 = [1, 5, 9, 2, 4]
dict1 = [{'a': 2}, {'a': 3}, {'a': 8}]
heapq.nlargest(2, list1)
heapq.nsmallest(4, list1)
heapq.nlargest(2, dict1, key=lambda x: x['a'])
```

## 19 统计列表中各元素出现的次数
```python
from Collections import Counter
a = [1, 2, 3, 2, 4, 2]
counter = Counter(a)
count_list = dict(counter)
print(counter.most_common(3))
```

## 20 isinstance()方法
Python中`isinstance(object, classinfo)`方法用于判断一个对象是否是一个已知的类型，与`type()`类似。它们的区别在于
>type() 不会认为子类是一种父类类型，不考虑继承关系。
>
>isinstance() 会认为子类是一种父类类型，考虑继承关系。
>
>如果要判断两个类型是否相同推荐使用 isinstance()。

对于classinfo的选择，可以是
`int`，`float`，`bool`，`complex`，`str`，`list`，`dict`，`set`，`tuple`。