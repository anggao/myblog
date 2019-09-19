---
title: "Python Note Three"
date: "2013-09-05"
template: "post"
draft: false
slug: "/posts/python-note-three/"
category: "python"
tags:
  - "python"
description: ""
socialImage: ""
---

* Iterator protocol
    + Objects that implements the `__iter__` method, `__iter__` returns an iterator
    object, which has a method called `next` (in PY3 `__next__`) return its "next value"
    And `StopIteration` raised when no more values to return.

```python
class Fib:
    def __init__(self):
        self.a = 0
        self.b = 1

    def next(self):
        self.a, self.b = self.b, self.a + self.b
        return self.a
    def __iter__(self):
        return self
```

* The built-in function `iter` can be used to get an iterator from an iterable object.

* A generator is a kind of iterator that is defined with normal function syntax. Any function contains a `yield`
statement is called a generator.

```python
def flatten(nested):
    try:
        try:
            nested +''
        except TypeError: pass
        else: raise TypeError
        for sublist in nested:
            for element in flatten(sublist):
                yield element
    except TypeError:
        yield nested
```

* Generator method
    - `send` works just like next, except it takes a single argument(the 'message' to send)
            `yield` may now used as an expression: `value = (yield oldvalue)`
    - rather than a statement, when the generator returned, `yield` returns a value, the value send from the 
    outside through send. If `next` was used, yield returns None.

* Eight Queens: state[0]=3 row 1 column 4 has a queen

```python
def conflict(state, nextX):
    nextY = len(state)
    for i in range(nextY):
        if abs(state[i]-next]]) in (0, nextY-i):
            return True

    return False


def queens(num = 8, state=()):
    for pos in range(num):
        if not conflict(state, pos):
            if len(state) == num-1:
                yield (pos,)
            else:
                for result in queens(num, state+(pos,)):
                    yield (pos,) + result
```

* Tell interpreter where to look module
```python
>>>import sys
>>>sys.path.append('test')
```


* "main program"  `__name__ == "__main__"`

* Pretty print
```python
>>>import pprint
>>>pprint.pprint(text)
```

* `__all__` variable: define the public interface of the module, tells the interpreter what it menas
to import all the names from this module: eg; `from copy import * `


* `__file__` property tells module location
