---
title: python装饰器
categories: Python
tags: Python 装饰器
icon: note
---

一般来说我们经常性会在工程性的代码里面经常性看到例如`@staticmethod`或是`@classmethod`等等，这就是装饰器，通常来说，装饰器的作用我们可以这样理解，它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。这样说着可能比较抽象，我们稍微接着看个很简单的例子就能很容易理解了。

## 1 关于装饰器
直接看一个很简单的例子 比如说我有一个`func()`函数，
```python
    def func():
        for i in xrange(100):
            print "bendawang"
```

我现在想要记录这个函数的执行时间。一般我们可能会这样写
```python
    import time
    def func():
        a=time.time()
        for i in xrange(100):
            print "bendawang"
        b=time.time()
        print str((b-a))+" s"
```
即记录下前后的时间，然后做个减法。 那如果我有100个和`func()`一样的函数`func1(),func2(),func3()......`，我要对每一个都记录执行时间怎么办呢？ 这样一个一个加代码肯定不划算啊，而且考虑到以后万一我又不想统计它们的时间，又还得一个个注释掉。 这里我们先看一种写法，
```python
    import time
    def get_time(myfunc):
        a=time.time()
        myfunc()
        b=time.time()
        print myfunc.__name__+" cost "+str((b-a))+" s"
    
    def func():
        for i in xrange(100):
            print "bendawang"
    
    get_time(func)
```   

这样我通过一个`get_time()`来间接调用`func(),func1(),func2()......`就能够成功记录时间了。 好的，能够简单的记录所有时间的问题解决了。

但是现在我们又面临了一个新的问题，比如我使用上面的方法，也就意味着我所有调用`func(),func1(),func2()......`的地方,现在都要换成`get_time(func),get_time(func1),get_time(func2)......`，但是我这是一个大的工程，要改那么多地方相当麻烦啊。

这个时候我们突然想到了，之前第一篇文章里面说过`python`一切皆对象，函数也是，那么我们的返回值也可以是一个函数咯，所以可以这样子写：
```python
    import time
    def get_time(myfunc):
        def wrapper():
            a=time.time()
            myfunc()
            b=time.time()
            print myfunc.__name__+" cost "+str((b-a))+" s"
        return wrapper
    def func():
        for i in xrange(100):
            print "bendawang"
    func=get_time(func)
    func()
```   

我们以一个函数为返回值覆盖原来的`func`，这样子我们就可以直接调用`func(),func1(),func2()......`，而且也能够记录时间也不用大动干戈去修改工程里面的其他代码。 这`get_time()`函数就可以叫做装饰器了，即装饰器的本质实际上就是一个函数而已，不过通常我们都通过`@`语法糖来简化装饰器的写法，上述代码可以这样子写：
```python
    import time
    def get_time(myfunc):
        def wrapper():
            a=time.time()
            myfunc()
            b=time.time()
            print myfunc.__name__+" cost "+str((b-a))+" s"
        return wrapper
    
    @get_time
    def func():
        for i in xrange(100):
            print "bendawang"
    func()
    
```
所以其实`@get_time`的本质就是`func=get_time(func)`。


## 2 关于参数问题
装饰器基本理解了，那么如果我们需要往函数里面传递参数怎么办？ 那其实这个问题是一样的，如下：
```python
    import time
    def get_time(myfunc):
        def wrapper(count):
            a=time.time()
            myfunc(count)
            b=time.time()
            print myfunc.__name__+" cost "+str((b-a))+" s"
        return wrapper
    
    @get_time
    def func(count):
        for i in xrange(count):
            print "bendawang"
    
    func(10)
```    

有一个就给`get_time`加一个参数，有两个就给它搞两个参数就好了。

那么这样子又有新的问题了。刚才说了，我有100个`func`函数，那么如果这些函数传入的参数不一样怎么办？怎么共用这个装饰器`get_time`？

那么我们通常装饰器的写法就出来了，利用`python`中的`*args, **kwargs`来引入可变参数 简单说一下这两兄弟的用法，`*args`表示任何多个无名参数，它是一个tuple；`**kwargs`表示关键字参数，它是一个 dict。并且同时使用`*args`和`**kwargs`时，必须`*args`参数列要在`**kwargs`前。 那么看一个例子就基本能理解了。
```python
    def foo(*args, **kwargs):
        print 'args = ', args
        print 'kwargs = ', kwargs
        print '---------------------------------------'
    
    if __name__ == '__main__':
        foo(1,2,3,4)
        foo(a=1,b=2,c=3)
        foo(1,2,3,4, a=1,b=2,c=3)
        foo('a', 1, None, a=1, b='2', c=3)
 ```

```python
    Result as follow:
    
    args =  (1, 2, 3, 4)
    kwargs =  {}
    ---------------------------------------
    args =  ()
    kwargs =  {'a': 1, 'c': 3, 'b': 2}
    ---------------------------------------
    args =  (1, 2, 3, 4)
    kwargs =  {'a': 1, 'c': 3, 'b': 2}
    ---------------------------------------
    args =  ('a', 1, None)
    kwargs =  {'a': 1, 'c': 3, 'b': '2'}
    ---------------------------------------
```
    

所以我们的装饰器可以这样写
```python
    import time
    def get_time(myfunc):
        def wrapper(*args,**kwargs):
            a=time.time()
            myfunc(*args,**kwargs)
            b=time.time()
            print myfunc.__name__+" cost "+str((b-a))+" s"
        return wrapper
    
    @get_time
    def func(count):
        for i in xrange(count):
            print "bendawang"
    
    func(10)
```    

所以其实我们经常在一些工程的项目里面都会看到例如下面的一些代码
```python
    def handleException(func):
        def _wrapper(args):
            try:
                out = Output()
                return func(args, out)
            except PenError as error:
                out.error(str(error))
            except ExploitError as error:
                out.error(str(error))
            except NotImplementError as error:
                out.error(str(error))
            except ORMError as error:
                out.error(str(error))
            except Exception as error:
                out.error(u"未知错误, '{0}'".format(error))
            except KeyboardInterrupt:
                out.error(u"用户强制退出")
            finally:
                out.close()
        return _wrapper
```    

然后工程中的很多函数都会用`@handleException`，相信很好理解了，就是对这些函数统一进行出错处理避免代码冗杂。这是工程中必不可少的思维，虽然我还没怎么写过工程性的东西。

## 3 关于多个装饰器的问题
因为可以使用多个装饰器，那么执行顺序就是问题，如下：
```python
    @get_time
    @test
    def func(count):
        for i in xrange(count):
            print "bendawang"
    
    func(10)
```    

上述代码我用了`get_time`和`test`两个装饰器，执行顺序是先执行`test`在执行`get_time`。

## 4 python-内置装饰器
这个算是本文的重要部分了，Python中有三个内置的装饰器，`staticmethod`、`classmethod` 和`property`。这些都是和类相关的，所以尽量看完第一篇再看这里。

首先说说`staticmethod`、`classmethod`，用文字说起来可能比较绕，用`staticmethod`和`classmethod`装饰的类中的函数实际上和一个全局函数差不多，可以通过类或是类的实例来调用，而一般的函数是不能通过类本身来调用，只能通过类的实例来调用。而这两个装饰器的差别在于，`classmethod`会将类本身当作第一个参数传入,即可以获得当前类的属性和方法。看一个例子
```python
    class Foo:  
        @staticmethod  
        def bar1(hello):  
            print "this is the staticmethod===>"+hello+"\n"
    
        @classmethod  
        def bar2(cls,hello):  
            print "this is the classmethod ===>"+hello+"\n"
    
        def bar3(self,hello):  
            print "this is a normal method ===>"+hello+"\n"    
    
    Foo.bar1('test')
    Foo.bar2('test')
    Foo.bar3('test')
`````    

结果如下：
```python
    this is the staticmethod===>test
    
    this is the classmethod ===>test
    
    Traceback (most recent call last):
      File "a.py", line 15, in <module>
        Foo.bar3('test')
    TypeError: unbound method bar3() must be called with Foo instance as first argument (got str instance instead)
```    

可以看出来第三个报错了，因为说过了普通函数只能用实例来调用，所以只能这样子写
```python
    class Foo:  
        @staticmethod  
        def bar1(hello):  
            print "this is the staticmethod===>"+hello+"\n"
    
        @classmethod  
        def bar2(cls,hello):  
            print "this is the classmethod ===>"+hello+"\n"
    
        def bar3(self,hello):  
            print "this is a normal method ===>"+hello+"\n"    
    
    Foo.bar1('test')
    Foo.bar2('test')
    foo=Foo()
    foo.bar3('test')
```    

接下来是`@property`，个人感觉这就是java程序员带过来的东西。。。先说这个装饰器吧，这里直接用了[廖雪峰官网关于@property的介绍](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386820062641f3bcc60a4b164f8d91df476445697b9e000)中的例子。

在绑定类属性时，如果我们直接把属性暴露出去，虽然写起来很简单，但是，没办法检查参数，比如如下：
```python
    s = Student()
    s.score = 9999
```    

如果我们想要限制`s.score`在一个范围怎么办，作为半个曾经的java程序员，我必然是会用eclipse的右键对这个`score`属性创建一个`getter`和`setter`方法，即在调用的时候不直接`s.score`，而是用getter方法，在修改的时候不直接赋值，而是用`setter`方法，从而避免属性暴露。用python 写就是这样子：
```python
    class Student(object):
    
        def get_score(self):
            return self._score
    
        def set_score(self, value):
            if not isinstance(value, int):
                raise ValueError('score must be an integer!')
            if value < 0 or value > 100:
                raise ValueError('score must between 0 ~ 100!')
            self._score = value
```    

但是这样子的话就很麻烦，每一次调用都要手动调用一次`get_score`函数，每次修改都要手动调用一次`set_score`函数，当然java这门语言本身就很麻烦所以再麻烦一点也没啥关系了，但是python不一样啊。所以有了这个`@property`，如下：
```python
    class Student(object):
    
        @property
        def score(self):
            return self._score
    
        @score.setter
        def score(self, value):
            if not isinstance(value, int):
                raise ValueError('score must be an integer!')
            if value < 0 or value > 100:
                raise ValueError('score must between 0 ~ 100!')
            self._score = value
```    

把一个`getter`方法变成属性，只需要加上`@property`就可以了，此时，`@property`本身又创建了另一个装饰器`@score.setter`，负责把一个`setter`方法变成属性赋值。 当然如果这个变量不叫`score`，比如叫`point`，那么另一个装饰器自然就变成了`@point.setter`。 这样子我们就可以这样操作了：
```python
    >>> s=Student()
    >>> s.score=50
    >>> s.score
    50
    >>> s.score=1000
    Traceback (most recent call last):
      File "<input>", line 1, in <module>
        s.score=1000
      File "<input>", line 12, in score
        raise ValueError('score must between 0 ~ 100!')
    ValueError: score must between 0 ~ 100!
```    

所以有了这个`@property`，我们在对实例属性操作的时候，就知道该属性很可能不是直接暴露的，而是通过getter和setter方法来实现的。

当然python里面除了本文介绍的函数装饰器之外，还有类装饰器，但是个人遇到的较少所以此处不介绍了。

## 5 关于装饰器的其他内容
1. 在使用装饰器后， 调用函数的`__name__`和`__doc__`属性，发现不再是原函数的名称和文档，而是装饰器函数wrapper的名称和文档。为了在装饰后保持原函数的名称和文档，需要在`def wrapper()`前加上另一个装饰器：`@wraps(func)`。具体使用方法为：
```python
from functools import wraps   
def my_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        '''decorator'''
        print('Calling decorated function...')
        return func(*args, **kwargs)
    return wrapper 
```
 wrapper 
```
