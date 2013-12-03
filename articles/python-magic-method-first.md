Python类的魔法之一——创建类的实例
=======

我准备用几篇文章来介绍Python的魔术方法，首先说Python创建类的实例的过程。
在开始前，我们先来回顾下Python类实例的属性：

```python
>>> s = 'hello'  
>>> dir(s)  
['__add__', '__class__', '__contains__', '__delattr__', '__doc__', '__eq__', '__ format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__get  slice__', '__gt__', '__hash__', '__init__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__',  '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '_formatter_field_name_split', '_formatter_parser', 'capitalize', 'center', 'count', 'decode', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'index ', 'isalnum', 'isalpha', 'isdigit', 'islower', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']  
```

在上面的例子里面，定义了一个字符串s。然后使用内建的 dir( ) 方法来看这个实例有哪些属性。

注意输出里面出现的前后有两个下划线的方法，这些是Python的魔术方法，也是接下来要主要关注的。

我们首先要关注的有3个方法，__new__ 、__init__和__del__。

首先这里面最熟悉的应该是就是 __init__ 方法了，这也是我们在定义类的时候最常用的方法__init__ 一般被当作是类的构造函数。不过实际上构造首先会调用 __new__ 方法，由__new__ 方法来构造一个对象，然后返回对象，由__init__来进行对象实例的初始化。不过大部分情况下__init__就够用了。

上面介绍了构造函数，那么 __del__ 就是析构函数了，顾名思义，这是在对象被执行垃圾回收的时候执行。在这里要注意 x.__del__( ) 并不实现del x的操作。在Python的内存管理中，有一个针对对象实例的计数器，只有计数器的计数变为0的时候，才会执行垃圾回收工作。所以计时一段Python程序执行结束，并不保证里面的对象实例的垃圾回收操作被触发。

下面用一个例子来说明：

```python
class Test(object):  
  
    def __new__(cls,):  
        print 'in new method'  
        return object.__new__(cls)  
  
    def __init__(self):  
        print 'in init method'  
  
    def __del__(self):  
        print 'in del method'  
  
if __name__ == '__main__':  
  
    a = Test()
```

这段代码的执行结果为：

```python
$ python test.py
in new method  
in init method  
in del method  
```