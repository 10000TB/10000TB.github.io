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

**Example 5: More ways to create a generator object**  
  
I already covered one common way with list comprehension - use `()` in list comprehension syntax.  

One other way - using `yield` keyword:  

```python
In [17]: def my_range_generator(n):
    ...:     i = 0
    ...:     while i < n:
    ...:         yield i
    ...:         i = i + 1
    ...:

In [18]: gen = my_range_generator(4)

In [19]: print gen
<generator object my_range_generator at 0x35b5230>

In [20]: for i in gen:
    ...:     print i
    ...:
0
1
2
3

In [21]: for i in gen:
    ...:     print i
    ...:

In [22]: 

```

**Example 6: Generator is a pattern**  
  
To tackle problems like following:  
  1. You have a long list of objects to iterate through, and they won't fit in memory all at once.
  2. You only need to access objects in a sequence once to do calculation, and you want to save memory.
  
Then the **generator pattern** come in as a way to generate/calculate next value on the fly, which save the expensive memory.  
  
At the beginning, I introduced that you can create a generator with `()` with list comprehension fashion. However, there are more ways than that.
In fact, you can create your own customized generator class. Using `()` and `yield` are just the built-in convenience provided by python for us to take advantage of
generator pattern in an easy way.  

With that being said, lets see an fully self-defined generator:  

```python
In [28]: class my_customized_range_generator(object):
    ...:     def __init__(self, n):
    ...:         self.idx = -1
    ...:         self.max = n
    ...:     def __iter__(self):
    ...:         return self
    ...:     def next(self):
    ...:         self.idx += 1
    ...:         if self.idx >= self.max:
    ...:             raise StopIteration
    ...:         return self.idx
    ...:

In [29]: gen = my_customized_range_generator(4)

In [30]: for i in gen:
    ...:     print i
    ...:
0
1
2
3

In [31]: print gen
<__main__.my_customized_range_generator object at 0x3b11bd0>

In [32]: for i in gen:
    ...:     print i
    ...:

In [33]:
```
As you can observe from the code snippet above, the behavior of it is same as the generator we created above: it is an iterable and it does not preserve items in memory, and it can only be iterated once. 

You may ask why the printed info does not show that the object is an generator. Well, since we are self-defining a generator according to generator pattern, python itself does not show it is a generator. The reason why two previous ways does is that they are using the built-in feature keyword/syntax, and python categorizes objects created in those fashion as generator. But, indeed, the object created above is also a generator. With all those being said, generator is a pattern to solve problems like I listed. `()` and `yield` are just python's built-in support for making your life easier to use it instead of talking hassle to create a generator class.
  
Those are all examples I had in mind and that I thought are useful in understanding generator. Comments warmly welcomed.
