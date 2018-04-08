---
layout: "post"
title: "Difference between eval, exec in python"
comments: true
categories: python
tags: [python, blogseries, eval, exec, compile]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy-style2.png"
---

I first approached code/expression executation/evaluation in python when I trying to 
assign values to variables of dynamically constructed names. Lately, I have run into 
a new scenario when I need to evaluate a dynamically constructed expression within python. 
I could go straight to what I did before to solve the immediate problem,  
  
However, out of curiousity, I want to take a moment to read, and learn what are other ways python has that allow us to execute code within normal code. This, I strongly believe, is very beneficial as not only does it teach me how to solve such problems in more ways, but also it gives me a bigger picture of python's built-in support for such problems, which eventually helps making a better decision on which is a best way, and accumulating experience on general python practices.  
  
To put it in a simple way, `eval()` is used to evaluate a single expression, and return value if there is any. Furthermore, `exec()` is a more powerful way, and it executes a statement or a program. Finally, when we talk about `eval` and `exec`, we usually need to know `compile()`. `compile()` is a built-in function compile program/code into code object, which can then be passed to `exec()` for execution.  There are many details behind each function like what kind of type/format can code/expression be presented to these functoins, but for first impression, I think above is enough in terms of what they do and how they are different.  
  
To elaborate more on details:  
  
- [x] `eval(expression[, globals[, locals]])`  
  The expression can be in Unicode or Latin-1 encoded string.  
  
  Notes:  
  - globals, locals if provided should be dictionary, and any mapping object respectively.  
  - globals, locals if provided will be used as the globals and locals namespace for the expression.  
  - Normally, the expression has full access to the standard `__builtin__` module.  
  - `eval` cannot execute statement(s), and statements should go to `exec()`.  
  - execution from file is supported by the built-in `execfile()`.  
  - built-in modules available to the code can be controled by pass a `__builtin__` key to globals with available builtins.
  - A simple example using `eval`:  

  ```python
  In [19]: a = 3
  
  In [20]: b = 4
  
  In [21]: op = '+'
  
  In [22]: eval(str(a)+op+str(b))
  Out[22]: 7
  ```

- [x] `exec(object[, globals[, locals]])`   
  This dynamically executes python code. What is different from `eval` is that this executes real code instaed of simple expression.
  object can either be a string, or a code object.
  
  Notes:
  - return value is None.  
  - An example of executing a series of statements with `exec`:   

  ```python
  In [5]: exec("""
     ...: a = 3
     ...: b = a
     ...: print b
     ...: """)
     ...:
  3
  
  In [6]:
  ```
  - The variables created are still kept within current namespace, meaning after we `exec`, we can access variables created within.  
  - built-in modules available to the code can be controled by pass a `__builtin__` key to globals with available builtins.


reference: https://stackoverflow.com/questions/2220699/whats-the-difference-between-eval-exec-and-compile-in-python
