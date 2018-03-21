---
layout: "post"
title: "A curated list: those python How-Tos we search frequently in our daily programming"
comments: true
categories: python
tags: [python, blogseries, dummynotes, cureated-list]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---

>How to count occurrences of a char in a string?; how to sort a list?; how to iterate through a list with access to index?, etc.

Often times, when we work with different languages, our attentionn is spreaded and don't usually remember good ways of doing something in a particular language at times. Like quoted above, we search for simple syntatic meat, or language sugar as such as a result on a regular basis if not frequent. With this post, I am collecting, organizing and looking after a list of such things in python.

Bookmark this page for your references !  
Comment below to contribute your examples !


- [x] What is `zip` founction in python ?  
  It takes a number iterables and return a list of tuples with ith tuple's values from ith element from each iterable.  
  ```
  In [34]: lst1 = ['hah', 'ad', 'c', 'd']
  
  In [35]: lst2 = [1, 2, 3, 4]
  
  In [36]: lst3 = [1.2, 4.4, 7.1, 1.1]
  
  In [37]: zip(lst1, lst2, lst3)
  Out[37]: [('hah', 1, 1.2), ('ad', 2, 4.4), ('c', 3, 7.1), ('d', 4, 1.1)]
  
  In [38]:
  ```
  If iterable's elements are of different size, `zip` will go most to last element of smallest iterable.  
  `zip` will always create the tuple in the order of iterables from left to right.  
    
  To unzip a list of tuples, we can call `zip` again by prefixing the list of tuples with `*`(star); we will get separate lists:  
  ```
  In [38]: tuples = zip(lst1, lst2, lst3)
  In [41]: zip(*tuples)
  Out[41]: [('hah', 'ad', 'c', 'd'), (1, 2, 3, 4), (1.2, 4.4, 7.1, 1.1)]
  ```


- [x] How to create a dictionary with default value(s) ?  
  with `defaultdict` from `collections`, we can achieve that:  
  ```python
  from collections import defaultdict

  data = [('Mon', 10.0), ('Tue', 21.5), ('Wed', 22.5), ('Thur', 17.5), ('Fri', 29.2)]
  spending = defaultdict(float)
  for day, dollar in data:
      spending[day] = dollar
  # defaultdict(<type 'float'>, {'Fri': 29.2, 'Thur': 17.5, 'Wed': 22.5, 'Mon': 10.0, 'Tue': 21.5})
  ```
  Another example of using `defaultdict` to consolidate a list of tuples:  
  ```
  data = [('A', 2), ('B', 23), ('C', 1), ('d', 100), ('a', 99), ('b', 5), ('c', 12)]
  combined = defaultdict(list)
  for k, v in data:
      combined[k.lower()].append(v)
  # defaultdict(<type 'list'>, {'a': [2, 99], 'c': [1, 12], 'b': [23, 5], 'd': [100]})
  ```

- [x] What is `dir()` function in python ?  
  As from python docs:  
  `dir([object])`  
  Without arguments, return the list of names in the current local scope. With an argument, attempt to return a list of valid attributes for that object.  
  ```python
  In [1]: dir()
  Out[1]:
  ['In',
   'Out',
   '_',
   '__',
   '___',
   '__builtin__',
   '__builtins__',
   '__doc__',
   '__name__',
   '_dh',
   '_i',
   '_i1',
   '_ih',
   '_ii',
   '_iii',
   '_oh',
   '_sh',
   'exit',
   'get_ipython',
   'quit']
  
  In [2]: class Foo(object):
     ...:     pass
     ...:
  
  In [3]: f = Foo()
  In [4]: dir(f)
  Out[4]:
  ['__class__',
   '__delattr__',
   '__dict__',
   '__doc__',
   '__format__',
   '__getattribute__',
   '__hash__',
   '__init__',
   '__module__',
   '__new__',
   '__reduce__',
   '__reduce_ex__',
   '__repr__',
   '__setattr__',
   '__sizeof__',
   '__str__',
   '__subclasshook__',
   '__weakref__']
  
  In [4]:
  ```
  If a object has a function named: `__dir__()`, this method will be called and must return a list of attributes. For example:  
  ```python
  In [6]: class Shape(object):
     ...:     def __dir__(self):
     ...:         return ["attr1", "attr2", "attr3"]
     ...:
  
  In [7]: s = Shape()
  
  In [8]: dir(s)
  Out[8]: ['attr1', 'attr2', 'attr3']
  
  In [9]:
  ```
  Every object in python has attributes.  
  ```
  In [1]: a = 3
  
  In [2]: dir(a)[:5]
  Out[2]: ['__abs__', '__add__', '__and__', '__class__', '__cmp__']
  
  In [3]: b = (23,2,3)
  
  In [4]: dir(b)[:5]
  Out[4]: ['__add__', '__class__', '__contains__', '__delattr__', '__doc__']
  
  In [5]: str_ = 'test'
  
  In [6]: dir(str_)[:5]
  Out[6]: ['__add__', '__class__', '__contains__', '__delattr__', '__doc__']
  ```
  To get value for each attribute, use `getattr` or dot syntax:  
  ```
  In [7]: str_.__add__
  Out[7]: <method-wrapper '__add__' of str object at 0x1078f6690>
  
  In [8]: getattr(str_, '__add__')
  Out[8]: <method-wrapper '__add__' of str object at 0x1078f6690>
  ```
  Dot syntax(`.`) is nothing more than syntactic sugar for `getattr()` in python.  

  Advantage for dot notation: It is easier to read.  

  Advantage for `getattr()`: Allow getting attributes by dynamically constructed string.  

  Correspondingly, `setattr()` and dot notation with assignment are same thing.
  
  In python, we can create and assign a new attribute value by … well, by simply assigning to it.  
  
  We can assign new attributes to nearly any object in Python.  
  ```
  def hello():
      return "Hello"
  
  >>> hello.abc_def = 'hi there!'
  
  >>> hello.abc_def
  'hi there!'
  # Credit: http://blog.lerner.co.il/python-attributes/
  ```
  (Python functions are objects as well.)  
  
  "we’re not creating variables at all. Rather, we’re adding one or more additional attributes to the particular object (i.e., instance) that has been passed to __init__."  
  * No difference between `self.x = 3` inside `__init__`  and `f.x = 3` outside. Doing it inside instead helps make code more readable, more convenient.

  > "This is one of those conventions that is really useful to follow: Yes, you can create and assign object attributes wherever you want. But it makes life so much easier for everyone if you assign all of an object’s attributes in __init__, even if it’s just to give it a default value, or even None. "

  Difference between function body and class body:  
  > A function’s body is only executed when we invoke the function. However, a the body of the class definition is executed immediately, and only once — when we define the function. We can execute code in our class definitions:  
  
  ```
  class Foo(object):
    print("Hello from inside of the class!")
  # We should never do this, but this is a byproduct of class definition executes immediately.
  ```

  Creating a class attribute:  
  ```
  class Foo(object):
      x = 100
  # This is not creata a class variable and assign a value, but create a class attribute and assign a value to it.
  ```

- [x] Why python class methods need a `self` as first argument ?  
  Standardalone definition of founction in current scope is usually global. When we define a founction within a class, it is actually creating an attribute with founction name in that class.  
  ```
  >>> class Foo(object):
          def blah(self):
              return "blah"
  
  >>> Foo.blah
  <unbound method Foo.blah>
  ```
  In fact, class founctions in python sit on class instead of sitting on instance. When we invoke founction through instance(f), python is actually invoking it from class()(Foo) and pass instance as reference(f).  
  
  There is no difference between `f.blah()` and `Foo.blah(f)`.  

  Reference: [1](http://blog.lerner.co.il/python-attributes/)

- [x] Check if two sets have intersection in python ?
  ```python
  In [1]: a = set(['1', '2', '3'])
  
  In [2]: b = set(['3', '4', '5'])
  
  In [3]: a.isdisjoint(b)
  Out[3]: False
  
  In [4]: c = set(['4', '5', '6'])
  
  In [5]: a.isdisjoint(c)
  Out[5]: True
  
  In [6]:
  ```

- [x] How to format string with auto-filled zeros in front to satisfy required amount of digits?
  with string `format`:
  ```
  In [1]: format(1, "02")
  Out[1]: '01'

  In [2]:
  ```

- [x] How to print type of a variable in python?
  with `type()` in python
  ```python
  In [1]: a = 'test'
  
  In [1]: print type(a)
  <type 'str'>
  
  In [2]: b = 1
  
  In [3]: print type(b)
  <type 'int'>
  
  In [4]:
  ```

- [x] How to get base-10 value of a hexdecimal digit ?
  with built-in function `int()`, we can do:
  ```python
  In [1]: int('a', 16)
  Out[1]: 10
  
  In [2]: int('b', 16)
  Out[2]: 11
  ```
  A little hack here: in fact, `int(val, base)` is a calculation mechanism, take a look at some examples I tried:  
  ```
  In [3]: int('a', 15)
  Out[3]: 10
  
  In [4]: int('a', 14)
  Out[4]: 10

  In [5]: int('a',11)
  Out[5]: 10
  
  In [6]: int('a',10)
  ---------------------------------------------------------------------------
  ValueError                                Traceback (most recent call last)
  <ipython-input-99-fb0c9de8bbed> in <module>()
  ----> 1 int('a',10)
  
  ValueError: invalid literal for int() with base 10: 'a'
  
  In [7]:
  ```

- [x] How to increment a char in python?
  with `ord()` returning unide code point of a single char when it is an unicode object and `chr` return string representation of a char by its unide code integer.
  ```python
  # get next char.
  In [1]: chr(ord('a')+1)
  Out[1]: 'b'

  In [2]: chr(ord('b')-1)
  Out[2]: 'a'
  ```

- [x] Add color to text in python?
  Ref: [1](http://ozzmaker.com/add-colour-to-text-in-python/)

- [x] How to create a new set with a single string in it ?  
  Pass a list with single string at parameter for `set()`:
  ```python
  In [1]: set(["teststring"])
  Out[1]: {'teststring'}
  ```
  **Caution:** when you simply pass a string to `set()`, you will end up with a set of all chars in the string:
  ```python
  In [2]: set("teststring")
  Out[2]: {'e', 'g', 'i', 'n', 'r', 's', 't'}

  In [3]:
  ```

- [x] How to make a list of n zeros ?  
  with `[0] * n`
  ```python
  In [1]: [0] * 4
  Out[1]: [0, 0, 0, 0]
  
  In [2]: [0] * 10
  Out[2]: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
  ```

- [x] How to return index of first appearance of a substring in a string ?  
  with `str.find(substring)`:
  ```python
  In [7]: a='I am david hu am am'
  
  In [8]: a.find('am')
  Out[8]: 2
  In [9]: a.find("john")
  Out[9]: -1
  ```

- [x] How to do float division in python ?  
  Example:  
  ```python
  >>> a = b = 3
  >>> a/b
  1
  >>> a/float(b)
  1.0
  >>>
  ```

- [x] How to check if a string is of (positive, unsigned) integer ?  
  with `str.isdigit()`:  
  ```python
  >>> str_val = "12312"
  >>> str_val.isdigit()
  True
  >>> str_val = "123.23"
  >>> str_val.isdigit()
  False
  >>> str_val = "adad"
  >>> str_val.isdigit()
  False
  ```

- [x] How to check if a string can be convert to float?  
  A simple and working way:  
  ```python
  try:
      float(val_str)
  except ValueError:
      print "not a float"
  ```

- [x] How to enter multiline string in interactive shell ?  
  A simple way with `"""`:
  ```python
input_str = """
  ```
  once you enter `"""`, it will let you `enter` to enter newline to construct multiline string. It will end when you enter a enclosing `"""`. It will look like following after you enter:
  ```python
input_str = """
...   test multiline string
...  one more line
... one more """
>>>
  ```

- [x] One-liner to search and get a line by a target string in python?  
  A straightforward way:  
  ```python
>>> input_str = """
...    I am a test
...   haha
... target this is a target line
... I am a target line
... fake line
... why cant I be a target line?
... """
>>> [line for line in input_str.split("\n") if "target" in line]
['target this is a target line', 'I am a target line', 'why cant I be a target line?']
>>>
  ```

- [x] How to define custom exceptions in python ?  
  A bare minimum example:  

  ```python
  class MyException(Exception):
      pass

  raise MyException("Something went wrong error")
  ```

  This will produce a traceback like: `MyException: Something went wrong error`
  
  To override something(i.e. for passing extra args when raising an exception), do this:  

  ```python
  class MyException(Exception):
      
      def __init__(self, message, extra_args):
          super(MyException, self).__init__(message)
          # Custom code here to handle extra args.
          self.extra_args = extra_args
  ```

  The benefit of this is that we can pass in a dictionary like `extra_args`, and retrieve it later once thrown with
  `e.extra_args` (given you catch exception like this: `except MyException as e:`)
  
  Note in python 3, we can use `super().__init__(message)` instead of `super(MyException, self).__init__(message)`.  
  **Reference link:** [[1]](https://stackoverflow.com/questions/1319615/proper-way-to-declare-custom-exceptions-in-modern-python)

  &nbsp;

- [x] What is "late binding" in python ?  
  An example: imports that happen during runtime.  
  Late binding of imports is fortunately rarely done (its is slow and against PEP-8 `Python Enhancement Proposals`) 

  &nbsp;

- [x] How to **count occurrences of a char/substring** in a string in python?  
    
  simple way:  
  ```python
  >>> str_val = "I am an example string"
  >>> str_val.count('a')
  3
  >>>
  ```
  <details>
      <summary>Details</summary>
      <p>
          <u>Syntax</u>: <Strong>str.count(sub[, start[, end]])</Strong> Return the number of non-overlapping occurrences of substring sub in the range [start, end]. Optional arguments start and end are interpreted as in slice notation.<br/>
          <u>references:</u> <a href="https://stackoverflow.com/questions/1155617/count-the-number-occurrences-of-a-character-in-a-string">[1]</a>
      </p>
  </details>
  &nbsp;

- [x] How to **reverse a string** in python?  
  A simple way with python's extend slicing:  
  ```python
>>> str_val = "I am David"
>>> str_val[::-1]
'divaD ma I'
>>>
  ```
  

  &nbsp;

- [x] What is python's **extended slices** syntax?  
  Since python 1.4, python's slicing syntax supports an optional third argument: `step` or `stride`.  
  Examples:  

  ```python
>>> lst = [1, 2, 3, 4]
>>> lst[0::1]
[1, 2, 3, 4]
>>> lst[0::2]
[1, 3]
>>>
>>> str_val = "This is my example of demonstrating extended slicesw"
>>> str_val[::2]
'Ti sm xml fdmntaigetne lcs'
>>> str_val[::-1]
'wsecils dednetxe gnitartsnomed fo elpmaxe ym si sihT'
>>>
  ```
  
  &nbsp;

- [x] How to convert **string to list** and how to convert **list to string** in python?  
  string to list:  
  ```python
>>> val_str = "IamDavid"
>>> list(val_str)
['I', 'a', 'm', 'D', 'a', 'v', 'i', 'd']
>>>
  ```
  list to string:  
  ```python
>>> val_lst = ['I', 'am', 'David']
>>> ''.join(val_lst)
'IamDavid'
>>>
  ```
  **caution**: _if item(s) in the list is not of type str, you need to convert them into string in order to have `join` work, for example:_  
  ```python
>>> val_lst = [1, 2, 3, 4]
>>> ''.join(str(val) for val in val_lst)
'1234'
>>>
>>> ''.join(val for val in val_lst)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: sequence item 0: expected string, int found
>>>
  ``` 
  **syntax**:   
  `str.join(iterable)`  
  Return a string which is the concatenation of the strings in iterable. A TypeError will be raised if there are any non-string values in iterable, including bytes objects. The separator between elements is the string providing this method.
 
  &nbsp;

- [x] What does the error: `TypeError: 'str' object does not support item assignment` mean ?  
  String is immutable in python as a design choice by makers of python.  
  Python would throw this exception when you try to mute part of a string. A simple reproducing example could be:  
  ```python
>>> str_val = "I am A 23 *"
>>> str_val[-1] = "&"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
>>> 
  ``` 

  &nbsp;

- [x] How to remove/strip an exact substring from end of a string ?  
  A simple way with slicing:  
  ```python
>>> str_val = "I am David Hu"
>>> str_rmv = " Hu"
>>> str_val[:-len(str_rmv)]
'I am David'
>>>
  ```
  `str.strip(...)` will also work given the pattern, but be cautious and you might get unexpected result.
  
  &nbsp;

To curate:  
- [ ] Explore python `collections`
- [ ] Python's variable scoping ?
- [ ] The time complexity for python to return the length of a string ?
- [ ] The time complexity for python to return the length of a list ?
- [ ] The time complexity for python to return the count of keys in a dictionary ?
- [ ] What is the error: `TypeError: 'unicode' object does not support item assignment` ?  References:[[1]](https://stackoverflow.com/questions/21416460/typeerror-unicode-object-does-not-support-item-assignment-in-dictionaries) 
- [ ] How do you run a shell command from python ?  
- [ ] What is python's native way of listing all files in a directory ? References: [[1]](https://stackoverflow.com/questions/3207219/how-do-i-list-all-files-of-a-directory)

