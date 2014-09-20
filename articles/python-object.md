
用Python有一段时间了，最近遇到的一个问题引起了我的注意，是关于Python对象的属性。
在Python里，有 \_\_dict\_\_ 这样一个魔术方法，它的功能是返回一个对象的所有属性。在实际的开发中我发现了一个有趣的现象。
```Python
>>> class Test(object):
...     def __init__(self, name, value):
...         self.name = name
...         self.value = value
...
>>> a = Test('tim', 25)
>>> a.name
'tim'
>>> a.value
25
>>> a.__dict__
{'name': 'tim', 'value': 25}
>>>
```
这里输出的结果是完全符合预期的，现在我用另一种方式来定义。
```Python
>>> class Test(object):
...     name = 'tim'
...     value = 25
...
>>> a = Test
>>> a.name
'tim'
>>> a.value
25
>>> a.__dict__
dict_proxy({'__module__': '__main__', 'name': 'tim', 'value': 25, '__dict__': <attribute '__dict__' of 'Test' objects>, '__weakref__': <attribute '__weakref__'of 'Test' objects>, '__doc__': None})
```
换了一种定义的方式，可以看到输出稍有不同，但是仍在预期之内，即name和value的值在__dict__里被正确输出。

```Python
>>> class Test:
...     name = 'tim'
...     value = 25
...
>>> a = Test()
>>> a.name
'tim'
>>> a.value
25
>>> a.__dict__
{}
```
现在又换了一种方式来定义，发现了问题，定义于类里的属性没有被记录在\_\_dict\_\_里。我猜想这是Python的实现细节不同，前面两个类都集成了object，是新式类，而最后一个是老式类。老式类在使用中仍然包含一些Python的遗留问题。所以我在以后的工作中一定要避免使用，定义类至少要集成object，当然Python3已经解决了这个问题。不过Python3里全面应用还很远，了解老式类与新式类的区别非常重要。