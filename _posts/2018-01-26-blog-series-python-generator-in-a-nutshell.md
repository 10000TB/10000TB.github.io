---
layout: "post"
title: "Python generator in a nutshell - a curated summary"
comments: true
categories: python
tags: [blogseries, python, generator]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---

No crispy definition on what is generator, just examples and explanations.  
   
Examples are listed in an incremental fashion so the next one is one level above the previous one.  
  

**Example 1 : What is it and how to construct a simple generator?**  
  
```python
 In [1]: generator = (i for i in range(4))

 In [2]: print generator
 <generator object <genexpr> at 0x2cbdbe0>
```
It is an iterable like list, string, and dictionary. It is mainly used to compare with list, and it has its special advantage over list.  
There are multiple ways to construct a generator object. But simply, with parentheses `()` in a comprehension fashion, you can create a generator object.

>Take a note: normally, list comprehension is using brackets `[]`. For example: `list = [ i for i in range(4)] `
  
**Example 2: How to use it?**
  
```python
In [1]: generator = (i for i in range(4))

In [2]: print generator
<generator object <genexpr> at 0x2cbdbe0>

In [3]: for val in generator:
   ...:     print val
      ...:
      0
      1
      2
      3
```
Since is is an interable, we mainly want to access individual item in it - iteration! `for...in` simply do the work.  
Check out the code snippet above.
  
**Example 3: What is special about generator?**  
  
It can only be iterated once.  
```python
In [1]: generator = (i for i in range(4))

In [2]: print generator
<generator object <genexpr> at 0x2cbdbe0>

In [3]: for val in generator:
   ...:     print val
   ...:
0
1
2
3

In [4]: for val in generator:
   ...:     print val
   ...:

```
If you look at this incremental example: we created an generator, iterated it once and printed all elements in the generator. We then
try to iterate through the generator again, but end up printing nothing. That is because the generator is empty now.
  
**Example 4: Define generator in a more detailed way**  

```python
TypeError: 'Example 4' has no attribute '__codeexample__' :)
```
I said at the beginning that a generator is simply a iterable that has some special advantage over list. With the 3 exampels presented above, is is then ready so say:  

```A generator is a way to return a sequence of values on the fly without storing them in memory.```

As I mentioned above that generator is usually compared with list, but it has special advantage over list, I would now like to give more detailed summary of generator:  

```
1. As iterating through it, it is like a list to returning a sequence of values.  
2. The great advantage of generator is that it does not store all values in memory, but it generate/calculate those values on the fly.
3. The trade-off for the great advantage is that the sequence of values is only available once. 
```

(TODO: 10000tb) Finish the rest of this post.

