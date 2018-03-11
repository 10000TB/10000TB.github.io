---
layout: "post"
title: "Be in the know: pass by value or pass by reference"
comments: true
categories: python
tags: [blogseries, python, dummynotes]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---

> Without reading further, following three points are worth taking away.  

**python function calls are pass-by-object-reference**  
**normal primitives will only pass by reference**  
**objects will pass by reference pointers**  
  
  
  
  
I was coding a python solution to a simple algorithm proble. The thinking/write-up took 10% of the whole duration of solving it, the rest of the time was spent on figuring out if the values are being pass by value or reference. The problem/confusion is so outstanding that I feel like I don't know python, though I already worked with python for a few months for various projects at work. Then, I spent some time gooling and would like to share some notes on this particular language
feature within python.  
  
There is a summary online that I think make good sense in categorizing styles of value or reference passing in function calls:  
  
```
- Pass by value
- Pass by reference
- Pass by object-reference
```
and python is `pass by object-reference`.  
  
In my opinion, three examples can illustrate the point to the majority if not the most:  
  
*Example 1: normal primitives only pass by reference.*  
```python
>>> def example_func(a):
...     a = 2
...
>>> val = 3
>>> example_func(val)
>>> print val
3
>>>
```

*Example 2: objects will pass by reference.*
```python
>>> def example_func(lst):
...    lst[0] = 2
...
>>> test_lst = [1, 3, 4]
>>> example_func(test_lst)
>>> print test_lst
[2, 3, 4]
>>>
```

*Example 3: reference is like a box, reassignment doesn't work.*
```python
>>> def example_func(lst):
...     lst = [2, 3, 4]
...
>>> test_lst = [1, 3, 4]
>>> example_func(test_lst)
>>> print test_lst
[1, 3, 4]
```
**You may assume that test_lst will be re-assigned with [2, 3, 4]**, which was I assumed anyway. In fact, it is not. The way this function call (`example_func`) works is that it creats a local pointer `lst`, and it points to the same object as `test_lst` does in memory. When executing `lst = [2, 3, 4]`, **it updates what `lst` points to** instead of doing a pointed-value re-assignment.
  
So be in the know and pass the right value/object type as needed.
