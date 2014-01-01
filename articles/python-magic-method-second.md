Python类的魔法之一——定制类的属性
=======

在上一篇里，首先了解了Python如何管理对象的创建和删除。现在来了解如何利用魔术方法定制类的属性。
在Python，比较常见的使用getter和setter来获取属性或为属性赋值。其实我们还可以用魔术方法来定制类在获取属性或被赋值时的动作。
在Python里，有4个魔术方法是来处理属性的。\_\_getattr\_\_, \_\_setattr\_\_ ，\_\_delsttr\_\_ 和 \_\_getattribute\_\_。
首先来看\_\_setattr\_\_。这是用来获取对象属性时调用的魔术方法。也就是说，当你执行obj.attr的时候，实际上会调用obj.\_\_getattr\_\_('attr')。不过在使用这个方法的时候需要相当的小心，看看下面的代码。
```python
def __setattr__(self, name):
    return self.name

def __setattr__(self, name):
    return self.__dict__[name]
```
上面的代码里，两个用于获取对象属性的魔术方法。不过上面一个有点问题。因为在调用obj.attr = val 的时候会调用到 \_\_setattr\_\_这个魔术方法，像第一段代码这么写会导致循环调用引发异常。正确的写法应该是下面的一种，从对象的属性键值对里将需要获取的值取出来。
这个问题在定义有关对象属性的魔术方法时都要注意的地方。
在调用del obj.attr的时候会调用到\_\_delattr\_\_这个魔术方法，使用方法跟\_\_setattr\_\_差不过。下面主要来关注获取对象属性的魔术方法。
用于获取对象属性的魔术方法有两个：\_\_getattr\_\_ 和 \_\_getattribute\_\_。这两个方法都有其用处。在正常的情况下。比如调用obj.attr的时候，会首先调用\_\_getattribute\_\_这个方法，如果能获取到属性，则返回。如果没有该属性，则抛出异常。然后会执行\_\_getattr\_\_，主要的异常处理部分会在这个方法里处理。
下面的一个例子可以表现出有关属性的魔术方法是如何运行的：
```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

class Person(object):

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __setattr__(self, attr, value):
        print 'setting attribute'
        super(Person, self).__setattr__(attr, value)

    def __delattr__(self, attr):
        print 'deleting attribute'
        super(Person, self).__delattr__(attr)

    def __getattribute__(self, attr):
        print 'in getattribute'
        super(Person, self).__getattribute__(attr)

    def __getattr__(self, attr):
        print 'in getattr'
        raise Exception('Attribute not exists')

if __name__ == '__main__':

    a = Person('Tom', '23')
    a.age = '25'
    del a.name
    print a.name
```
上面代码的执行结果：
```python
$ python magic.py
setting attribute
setting attribute
setting attribute
deleting attribute
in getattribute
in getattr
Traceback (most recent call last):
  File "magic.py", line 31, in <module>
    print a.name
  File "magic.py", line 24, in __getattr__
    raise Exception('Attribute not exists')
Exception: Attribute not exists
```
从上面的例子里可以很清楚的看出有关属性的魔术方法的执行。