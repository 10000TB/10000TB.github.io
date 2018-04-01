---
layout: "post"
title: "Python's High-performance container datatypes"
comments: true
categories: python
tags: [blogseries, python, dummynotes]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---

I have been using `defaultdict` quite a lot as an substitute to `dict` for a while, and `Counter` as well. They both come from python's standard module `collections`, which was first introduced in `2.4`. Since I am very satisfied with the two, I thought to myself why not check out the rest of the container types in the module. So here I compile this post on python's `collections` module, the "High-performance container datatypes" as claimed.  
  
This post will briefly introduce what are the container datatypes in `collections` and maybe explain why there are of high performance.  
  
As of `2.7`, there are now five useful things in `collections`: `namedtuple()`, `deque`, `Counter`, `OrderedDict`, `defaultdict`.  
  
- [x] **Counter**  
  In a nutshell, it helps you do counting; value and its counting are in the form of key value pair.  
  Note that `Counter` is a subclass of `dict`, so that should clarify a bit on how countings are represented.  
  
  notes:  
  * There are three ways to initialize a Counter object: `Counter()`, `Counter({"k1":3, "k2":4})`, `Counter(k1=3, k2=4)`.  
  * Counter has a dictionary-like interface except that it return zero for a non-existing key instead of a key error.  
  * `elements()` - return an iterator repeating each key as many times as its count.  
    In comparison, `keys()` return a list with all keys in it.
    
    ```python
    In [17]: from collections import Counter
  
    In [18]: c = Counter(ad=1, adas=3, adqw=23)
    
    In [19]: c.elements()
    Out[19]: <itertools.chain at 0x10f97b8d0>
    
    In [20]: for i in c.elements():
        ...:     print i
        ...:
    adas
    adas
    adas
    ad
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    adqw
    
    In [22]: c.keys()
    Out[22]: ['adas', 'ad', 'adqw']
    
    In [23]:
    ```
  * Since `Counter` is a subclass of `dict`, it jas access to `keys()` to get a list with all keys in itself.  
  * `most_common([n])` - return n most common keys and their counts in the form of tuples in the counter.  
  * `subtract([iterable-or-mapping])` - it subtracts counts passed in from current counter. Counts in the result can be zero or negative.  
    In comparison, `update([iterable-or-mapping])` adds the couts passed in on top of existin counts.
  Both of them does not replace, but subtracting or adding.  
  * We can also do mathmatical operations on counter object: `+`, `-`, `&`, `|`.  

- [x] ****



Reference(s):  
1. https://docs.python.org/2/library/collections.html
  
