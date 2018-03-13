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

- [x] One-liner to search and get a line in python?  
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
- [ ] The time complexity for python to return the length of a string ?
- [ ] The time complexity for python to return the length of a list ?
- [ ] The time complexity for python to return the count of keys in a dictionary ?
- [ ] What is the error: `TypeError: 'unicode' object does not support item assignment` ?  References:[[1]](https://stackoverflow.com/questions/21416460/typeerror-unicode-object-does-not-support-item-assignment-in-dictionaries) 
- [ ] How do you run a shell command from python ?  
- [ ] What is python's native way of listing all files in a directory ? References: [[1]](https://stackoverflow.com/questions/3207219/how-do-i-list-all-files-of-a-directory)

