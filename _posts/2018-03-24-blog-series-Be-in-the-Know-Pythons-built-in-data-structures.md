---
layout: "post"
title: "A curated summary: Python's built-in data structures"
comments: true
categories: python
tags: [python, blogseries, dummynotes]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy-style2.png"
---

I have used a lot of built-in data structures here and there, and are getting to know something new on them gradually. However, these knowledge are in pieces and almost all of them are from search results on the internet, which adds a bit of uncertaintity to them. Then, I had this idea of compiling a post on curating a summary on python's built-in data structures for a systematic learning and verification of what I have learnt. In additoin, having a systematic overview of everything one is frequently helps his/her better understanding of what is known, and build correlations among them, and even reversely think back why those data structures were designed in such way.  
> As the official documentation of python has exhausted official details of each data structure as per release, there is no need for redundant repeatition of them here. This post will briefly touch each data structure, as well as pointing out frequently used built-in founctions inside each. In addition, I am also sharing my actual experience with them.
  
   
   
In summay, built-in data structures are as follow: `list`, `tuple`, `set`, `dict`, `string`  
  
- [x] **list**  
    It is like array in other language.  
    
    _Important Concepts:_  
    1. Built-in founctions:  
        Given a list, we should be free to most things, but python built-in some functions for convenience.  
        For example:  
        Add new item(s) with:  
          `append` - append at the end, even it is a list by itself.  
          `extend(L)` - add all items in a list to the enid. Equivalent2: `a[len(a):] = L`.   
          `insert(idx, x)` - in-position insertion. `a.insert(0, x)`, `a.insert(len(a), x)`
        Remove item(s) with:  
          `remove(x)` - remove first item whose value is x.
          `pop([i])` - remove item at the given idx and return it; by default: -1  
        Search idx of first item whose value is as specified:
          `index(x)`  
        Count occurrences of an item in the list:  
          `count(x)`  
        Sort:
           `list.sort(cmp=None, key=None, reverse=False)` - This is in-place operation, while `sorted` return sorted list.  
        `list.reverse()` - In-place reverse elements in the list.  
    2. Use list as stack.  
    3. Use list as Queue.  
    4. built-in tools:  
        todo
    5. List comprehension.  
    6. Nested List comprehension.  

- [x] **Tuples and Sequences**  
    Tuple is a standard sequence data type. Like list and string, they share similar properties like indexing, slicing operations.  
    
    _Important Concepts:_  
    1. Tuple is immutable - (`TypeError: Tuple object does not support item assignment` will be thrown when you try to mutate an item).  
    2. A tuple consists of a number of values, separated by commas.  
    3. Tuples are often surrounded by parathesis - so that nested tuples are interpreted correctly. In addition, it is necessary when tuples are within a larger expression.  
    4. Tuple vs List: a tuple usuallu contains heterogeneous elements, while list usually contains homogeneous elements.  
    5. To create an empty tuple, use a pair of parathesis: `empty_tuple = ()`.  
    6. To create a tuple with only one item, assign an item followed by a single comma: `single = "hey",`  
    7. Tuple packing: `t = 123, 23, 'hah'`; Values of the three on the right are packed into a tuple.
    8. Sequence unpacking:`a, b, c = t`.  
      `Sequence unpacking` requires a list of variables on the left-side to have the same number of value as the tuple(In fact, any sequence type).

- [x] **Sets**  
    A set is an unordered collection with no duplicate elements.  
    
    _Important Concepts:_  
    1. To create a set, use `{}` or `set()` (Note to pass a list to `set()`; When passed a string, all chars will be treated an items to add into set).  
    2. set also support comprehension style like list: `set_ = {e for e in some_othe_sequence}`; `a = {x for x in "abracadabra" if x not in "abc"}`  
    3. When passing a string to `set()`, it adds each char in the string as items in to set:  
    ```python
    In [44]: set("hey i am xuehao")
    Out[44]: {' ', 'a', 'e', 'h', 'i', 'm', 'o', 'u', 'x', 'y'}
    ```
    4. Set objects mathmatical operation like union, intersection, difference, and symmetric difference.  

- [x] **Dictionary**
    An useful ds buit in python, known as associative array in other language.  
    
    _Important Concepts:_  
    1. Dictionaries are indexed by keys. (While sequences are indexed by a range of numbers).  
    2. It is unordered set of key:value pairs.  
    3. Keys in dictionaries can be any immutable type: string, numbers.  
    4. Tuple can be key if it only contains immutable items (strings, numbers, tuples).  
    5. Keys are unique within one dictionary.   
    6. With `del`, we can delete a key: `del test["key1"]`.  
    7. `keys()` return a list of all keys used in a dictionary.  
    8. With `in`, you can check if a single key is in dictionary.  
    9. dict comprehension can be used to create a dictionary:  

    ```python
    In [49]: keys = ["haha", "hhe", "tada", "pow", "s"]    
    In [52]: values = [1.2, 3, 4, "biu", "t"]
    In [53]: dt = { k:v for (k, v) in zip(keys, values)}
    
    In [54]: dt
    Out[54]: {'haha': 1.2, 'hhe': 3, 'pow': 'biu', 's': 't', 'tada': 4}
    
    In [55]:
    ```

    Another example:  
    ``` python
    In [55]: { x:2*x for x in range(3)}
    Out[55]: {0: 0, 1: 2, 2: 4}
    
    In [56]:
    ```  


- [x] **Sequence Types**  
    There are 7 types belonging to sequence types categories:  
    
    `str`  
      String literals: "adad", 'adads' (In single or bouble quotes)
    `unicode`  
      Prety like `str`, but is specified with a preceding `u`.  
      For example: u"abcddas", u'adad'
    `list`  
      List is constructed with square brackets: `[]`  
    `tuple`  
      Tuple is constructed with comma operator (with or without parathesis)
    `bytearray`  
      Created with built-in `bytearray()`
    `buffer`  
      Created with built-in `buffer()`
    `xrange`  
      Created with built-in `xrange()`  
      Yes, with this, you can create a range generator object.
    
    (In comparison, there are other containers in python: `set` and `dict`)

    reference: https://docs.python.org/2/library/stdtypes.html#typesseq
    https://docs.python.org/2/library/stdtypes.html#sequence-types-str-unicode-list-tuple-bytearray-buffer-xrange  

- [x] **Module: collections** - High-performance container datatypes  
       
    reference: https://docs.python.org/2/library/collections.html#module-collections  

- [x] **Looping Technique**
    1. How to get key and value at the same time when looping over a dictionary ?  
    with `dict.iteritems()`:  

    ```python
    In [58]: for k,v in dt.iteritems():
        ...:     print "Key: %s, value: %s" % (k, v)
        ...:
    Key: tada, value: 4
    Key: s, value: t
    Key: haha, value: 1.2
    Key: hhe, value: 3
    Key: pow, value: biu
    
    In [59]:    
    ```
   
    reference: https://docs.python.org/2/tutorial/datastructures.html#looping-techniques


References:  
1. [Python data structures - docs.python.org](https://docs.python.org/2/tutorial/datastructures.html)
