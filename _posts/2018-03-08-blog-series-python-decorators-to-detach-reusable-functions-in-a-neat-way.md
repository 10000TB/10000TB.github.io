---
layout: "post"
title: "Get started with python decorators: detach and layerize teusable functions"
comments: true
categories: python
tags: [blogseries, python, dummynotes]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---
In this post, I will share my story with python decorator. Along the way, I would mainly share places I have seen as a pretty use of python decorators, and with a focus on evangelizing the adoption of python decorators.

**When I first met python decorator**

The first time I saw a thing like `@function_name(args...)` is when I was checking as python util script at work, and it was like:
```python
@pyutil.retry(3)
def some_function_name(args...):
    ... 
```
>It is clear and I bet you already guess it out that the `@pyutil.retry(3)` will help retry the `some_function_name(args...)` 3 times if it fails.  
  
Ever since, I have seen similar things all around the places:  
  
With python flask, you can develop your RESTful services easily with the decorators from flask module.
```python
from app import app

@app.route('/index')
def index():
    return "Hello, World!"
```
In python `unittest` module, when you want to skip a test:
```python
class ExampleTest(unittest.TestCase):
    
    ... # skip any setup in this example.

    @unittest.skip("Skip this test before tick A is fixed.")
    def test_some_scenario(self):
        ... # real test logic here.

    def test_some_other_scenario(self):
        ... # real test logic here.
```
The list can go on and on. But what was pasted above was mainly my primer on python decorator.  
  
**Keep decorators in mind and maybe adopt it**
  
Decorator, in a nutshel, is a way to take a function and extend its behavior without modifying its behavior.  
  
A simple example is:  
  
```python
def my_decorator(f):
    def wrapper_function():
        print "before calling"
        f()
        print "after calling"
    return wrapper_function

def my_function():
    print "In my_function"

decorated_func = my_decorator(my_function)

decorated_func()
```
It should produce output like following:  
```
before calling
In my_function
after calling
```
In real world development, there are many great scenarios when you can create and use decorators.  

For example, you are dealing with many test scenarios that share some common setup transactions like: drop tables,
delete some users, validate data, etc. It would good to write them into common functions, and it would great to put them
in format of decorators, and then for each test scenarios (take tests under python unittest for example), you simply decorate the test
with needed transaction:
```python
... // necessary import logic.

class SomeTest(unittest.TestCase):
    ... // Suppose you have these decorators implemented/imported by this point.
        // @drop_some_tables()
        // @validate_data

    @drop_some_tables()
    def test_scenario1(self):
        ... // test logic.

    @validate_data()
    def test_scenario2(self):
        ... // test logic.

    @delete_some_users()
    def test_scenario3(self):
        ... // test logic.

... // main function.
```
Then, writing a new test that might need some of those common setup/transaction will become greatly easy and fast given 
those decorators.  
  
Other examples may include but not limited to: retry logic(rety decorated function on failure for a certain amount of times), running duration report(print running time of decorated function), check login status (like in the context of web serices, we need certain functions only proceed when a user is logged in), etc.  
  
The list can go on and on, with the concept, you can name your waf of using decorator.  

Maybe-Useful links for reading more on decorators:  
1.[Primer on Python Decorators](https://realpython.com/blog/python/primer-on-python-decorators/)
