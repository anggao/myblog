---
title: "Python Note Two"
date: "2013-09-04"
template: "post"
draft: false
slug: "/posts/python-note-two/"
category: "python"
tags:
  - "python"
description: ""
socialImage: ""
---

* Polymorphism: even if you don't know what kind of object a variable refer
to, you may still able to perform operations on it, that will work differently
depending on the class(type) of the object

* Polymorphism enables you to call the methods of an object without knowing
its class (type of object). Encapsulation enables you to use the object
without worrying about how it's constructd.

* To make a method or attribute private (inaccessible from outside)
simply starts its name with two underscores

* Inside a class definition, all names beginning with a double underscores
are `translated` by adding a single underscore and the class name to the beginning.

* Names with an initial underscore are't imported with starred imports.

* `issubclass(SpamFilter, Filter)`
 
* `isinstance(obj, cls)`

```python
>>>SpamFilter.__bases__
(< class __main__.Filter at 0x171e40> ,)
```

* `obj.__class__` : find out which class an object belongs to 

* In multiple inheritance, the methods in the earlier classes override the methods
in the later ones.

* Object attributes
    + `getattr(object, name [, default])`
    + `hasattr(object, name)`
    + `setattr(object, name, value)`

* Explore built-in exceptions (in module exceptions)
```python
>>>import exceptions
>>>dir(exceptions)
```

* Cataching two exceptions with one block
```python
try:
    doFun()
except (E1, E2):
    backup()
```

* Catching the exception object:
```python
except (ZeroDivisionError, TypeError), e:
# PY3
except (ZeroDivisionError, Typeerror) as e:
```

* example
```python
while True:
    try:
        x = input('Enter x')
        y = input('Enter y')
        value = x / y
        print 'x / y is ', value 
    except Exception, e:
        print 'Invalid input ', e
    else:
        break
    finally:
        print 'Done'
```

* If you override the constructor of a class, you need to call the constructor 
of the superclass or risk having an object that isn't properly initialized.

* If you retrieve the method from the class directly, there is no instance to 
which to bind, such method is called unbound, you can supply any self you want.
```python
def __init__(self):
    Bird.__init__(self)
```

* New style classes, `super` is called with the current class and instance as 
its arguments. Any method called on the returned object, will be fetched from the
superclass. This method returns super object.
`super(SongBird, self).__init__()`

* In PY3, `super` can be called without any argument.

* sequence and mapping protocol
    + immutable
        - `__len__(sel))` if return `0` treat as `False` in boolean contxt, can overrite with `__nonzero__`
        - `__getitem__(self, key)` 
    + mutable
        - `__setitem__(self, key, value)`
        - `__delitem__(self, key)` this is called when someone uses the `del` statement on a part of the object

* For a sequence `x[-n]` is the same as `x[len(x) - n]`
if the key is of an inappropriate type: `TypeError`
if the index of a sequence outside the allowed range: `IndexError`

* Examples
```python
def checkIndex(key):
    if not instance(key, (long, int)): raise TypeError
    if key < 0: raise IndexError

class ArithmeticSequence:
    def __init__()elf, start = 0, step = 1):
        self.start = start
        self.step = step
        self.changed = {}

    def __getitem__(self, key):
        checkIndex(key)
        try:
            return changed[key]
        except KeyError:
            return self.start + self.step * key
    def __setitem__(self, key, value):
        checkIndex(key)
        changed[key] = value
```

```python
class CounterList(list):
    def __init__(self, *args):
        super(CounterList, self).__init__(*args)
        self.counter = 0
    def __getitem__(self, key):
        self.counter += 1
        return super(CounterList, self).__getitem__(key)
```

* The attributes that are defined through their accessors
are often called properties

* Property example
```python
class Rectangle(object):
    def __init__(self):
        self.width = 0
        self.height = 0
    def setSize(self, size):
        self.width, self.height = size
    def getSize(self):
        return self.width, self.height
    size = property(getSize, setSize)
```

* `property(fget, fset, fdel, doc)` returns a property, all arguments are optional

* `property` isn't really a function, it's a class whose instances have some methods
that do all the work. Those methods defines that so-called `descriptor`
protocol. Eg: if the attribute bound to an object that implements `__get__` method,
the object won't simply retuned, instead `__get__` method will be called and the resulting
value will be returned

* **static methods VS class methods**
    + static methods are defined without self arguments and they can be called
     directly on the class itself
    + class methods are defined with a self-like parameter normally called `cls`.
    You can call class methods directly on the class object too, but parameter then automatically is 
    bound to the class.

* `__getattr__` `__setattr__` and friends

  + `__getattribute__(self, name)` : Automatically called when the attribute name is called (only on new-style class)

  + `__getattr__(self, name)`: Automatically called when the attribute name is called and the object has no such attribute

  + `__setattr__(self, name, value)`: Automatically called when an attempt is made to bind the attribute name to value

  + `__delattr__(self, name)`

* Example
```python
class Rectangle:
    def __init__(self):
        self.width = 0
        self.height = 0

    def __setattr__(self, name, value):
        if name == 'size':
            self.width, self.height = value
        else:
            self.__dict__[name] = value

    def __getattr__(self, name):
        if name == 'size':
            return self.width, self.height
        else:
            raise AttributeError
```

* `__dict__` contains a dic with all the instance attributes

* There is a trap associated with `__getattribute__`, it intercepts all attributre
access, and `__dict__` as well. The only safe way to access attribute on self inside
`__getattribute__` is to use the `__getattribute__` method of the superclass

