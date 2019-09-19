---
title: "Python Note Four"
date: "2013-09-06"
template: "post"
draft: false
slug: "/posts/python-note-four/"
category: "python"
tags:
  - "python"
description: ""
socialImage: ""
---


* `sys` module 
    - `argv` command line arguments, including file name
    - `exit([arg])` exits the current program, optionality with a given number or error message.
    (if called within try/finally block, the `finally` clause is still executed)
    - `modules`: a dictionary mapping module name to loaded module
    - `path`: a list of directory names where modules can be found
    - `platform`: a platform identifier such as sunos/win32
    - `stdin` `stdout` `stderr`

* `os` module
    - `environment` : mapping with environment variables
    - `system`: executes an operation system command in a subshell
    - `sep` : separator used in path
    - `pathsep` : separator to separate paths
    - `linesep` : `\n` or `\r` or `\r\n`
    - `urandom(n)` : returns n bytes of cryptographicaly strong random data

* `fileinput` module
    - `input([files [, inplace [, backup]]])` facilities iteration over lines in multiple files
    - `filename()` : returns the name of the current file
    - `lineno()` : returns the current (cumulative) line number
    - `filelineno()` : returns the line number within current file.
    - `isfirstline()` : checks whether the current line is first in file.
    - `isstdin()` : checks wheather the last line was from `sys.stdin`
    - `nextfile()` : closes the current file and moves to next.
    - `close()` : close the sequence 

```python
# add line number to a file
import fileinput
for line in fileinput(inplace=True):
    line = line.rstrip()
    num = fileinput.lineno()
    print '%-40s #2i' % (line, num)
```

* **sets, heaps, deques**
    - sets
        + `issubset` `<=`
        + `issuperset` `>=`
        + `union`  `|`
        + `symmetric_difference` `^`
        + eg: reduce(set.union, mysets)
        + `frozenset` represents immutable sets. useful when you want to
        use a set either as a member of another set or as the key to a dictionary
    - heaps
        + `import heapq`
        + `heappush(heap, x)`
        + `heappop(heap)` pops off the smallest element
        + `heapify(heap)`
        + `heapreplace(heap, x)` pops off the smallest element and pushes x
        + `nlargest(n, iter)` returns the n largest elements of iter
        + `nsmallest(n, iter)` returns the n smallest elements of iter
    - deques
        + The `collections` module contians the `deque` type and `defaultdict`: a dictionary with a default 
        value for nonexisting keys.
```python
>>>from collections import deque
>>>q = deque(range(5))
>>>q.append(5)
>>>q.appendleft(6)
>>>q.pop()
5
>>>q.popleft()
6
>>>q,rotate(3)
```

* time
    - Time tuple: (2008,1,21,12,2,56,0,21,0)
    - last three values: weekday, (1-366), day light/savings(0,1,-1)
    - `asctime([tuple])`: convert a time tuple to a string.
    - `localtime([secs])` : converts seconds to a data tuple, local time
    - `mktime(tuple)` : converts a time tuple to local time
    - `sleep(secs)` : sleeps for secs seconds
    - `strptime(string[,format])` : parses a string into a time tuple
    - `time()` : current time (seconds since the epoch, UTC)

* random
    - `random()` returns a random real number `0 <= n < 1`
    - `getrandbits(n)` returns `n` random bits in the form of long integer
    - `uniform(a,b)` get a random number in the range `[a,b)` or `[a,b]` depending on rounding
    - `randrange([start], stop, [step])` returns a random number from range(start, stop, step)
    - `choice(seq)` returns a random element from sequence `seq`
    - `shuffle(seq [,random])` shuffle the sequence seq in place
    - `sample(seq, n)` choose n randoms, unique elements from the sequence seq.

* shelve : simple storage solution (persistent dictionary-like object)
```python
>>>import shelve
>>>s = shelve.open('test.data', writeback=True)
>>>s['x'] = ['a','b']
>>>s.close()
```

* `re` module
   - character sets: `[pj]` `[a-z]` `[^abc]`
   - alternatives and subpatterns: `p(ython|erl)`
   - optional and repeated subpatterns
        + `(pattern)*`
        + `(pattern)+`
        + `(pattern){m,n}`
   - compile(pattern [,flag]) : create a pattern object from a string with a regular expression
   - search(pattern, string [, flags]) : searchs for pattern in string
   - match(pattern, string [, flags]) : matches pattern at the beginning of string 
   - split(pattern, string [, maxsplit=0]) : splits a string by occurrences of pattern
   - findall(pattern, string) : returns a list of all occurrences of pattern in string
   - sub(pat, repl, string [, count=0]) : substitues occurrence of pat in string with repl
   - escape(string) : escaptes all special regular expression characters in string
   - **match objects and groups**
        + match object : contain information about the substring that matched the pattern
    also contian information about which parts of the pattern matched which parts of the substring. These parts 
    called groups.
        + group zero is the entire pattern
   - **important methods for match objects**
        + `group([group1,..])` retrieves the occurrences of the given subpatterns (groups)
        + `start([group])` : returns the starting position of the occurrence of a given group
        + `end([group])` : returns the ending position (an exclusive limit as in slices) of the occurrence of a given group
        + `span([group])` : returns both the beginning and ending positions of a group
```python
>>>m = re.match(r'www\.(.*)\..{3}', 'www.python.org')
>>>m.group(1)
'python'
>>>m.start(1)
4
>>>m.end(1)
10
>>>m.span(1)
(4,10)
```

```python
emphasis_pattern = re.compile(r'''
\*      # begining emphasis tag -- an asterisk
(       # begining group for capturing phrase
[^\*]+  # capturing anything except asterisks
)       # end group
\*      # end emphasis tag
''', re.VERBOSE)
re.sub(emphasis_pattern, r'<em>\1</em>', 'Hello, *world*!')
```

   - All the repetion operations can be made **nongreedy** by putting a question mark after them.
```python
r'\*\*(.+?)\*\*'
re,compile(r'[a-z\-\.]+@[a-z\-\.]+', re.IGNORECASE)
```

   - A simple template system:

```python
import fileinput, re
field_pat = re.compile(r'\[(.+?)\]')
scope = {}

def replacement(match):
    code = match.group(1)
    try:
        return str(eval(code, scope))
    except:
        exec code in scope
        return ''

lines = []
for line in fileinput.input():
    lines.append(line)
text = ''.join(lines)
print field_pat.sub(replacement, text)
```

   - `file` module
     + file mode
        * `r`, `w`, `a`
        * `b` : added to other mode
        * `+` : added to other mode, read/write mode
     + file like object, eg: urllib.urlopen()
     + `f.read(n)` : read `n` bytes/characters, or read all if no value provided.

```python
import sys
text = sys.stdin.read()
words = text.split()
wordcount = len(words)
print 'Word count : ', wordcount
```

   - random access
     + `seek(offset [, whence])`
       * default whence = 0, offset is form the beginning of the file, offset > 0
       * whence = 1, relative to current position, offset maybe negative
       * whence = 2, relative to the end of file
     + `tell()` : returns current file position

   - reading, writing lines:
     + `file.readline([n])` : optional number `n`, nonnegative integer, specify maximum bytes/characters to read.
     + `file.readlines()` : read all lines of a  file as a list
     + `file.writelines(seq)` : write all the string to the file, newlines are not added (use `os.linesep`)

   - **with statement**
```python
with open('somefile') as f:
    foo(f)
```

   - context managers : An object supports `__enter__` and `__exit__`, eg: `with` statement, takes no
   argument, called when entering `with` statement, return value is bound to the variable after as keyword.

   - `read` returns empty string when reached the end of file.
