---
layout: "post"
title: "Python generator in a nutshell"
comments: true
categories: python
tags: [blogseries, python, generator]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---

No crispy definition on what is generator, just examples and explanations.  
   
Examples are listed in an incremental fashion so the next one is one level above the previous one.  
  

**Example 1 : What is it? and how to construct a generator**  
  
```python
 In [1]: generator = (i for i in range(4))

 In [2]: print generator
 <generator object <genexpr> at 0x2cbdbe0>
```
With parentheses `()`, you create a generator object.  

>Take a note: normally, list comprehension is using brackets `[]`. For example: list = [ i for i in range(4)]

(TODO: 10000tb) Finish the rest of this post.
