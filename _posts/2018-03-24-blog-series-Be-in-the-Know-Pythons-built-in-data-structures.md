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
    

References:  
1. [Python data structures - docs.python.org](https://docs.python.org/2/tutorial/datastructures.html)
