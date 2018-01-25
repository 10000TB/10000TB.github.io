---
layout: "post"
title: "Suppressing python console stdout with context manager: from working solution to elegant solution"
comments: true
categories: python
tags: [blogseries, python, contextmanager, suppressconsoleout]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---

There have been more than once, as I can recall, when I need to suppress console output to achieve one goal or two. That makes it become natural from my blogging thinking: why don't I compile a post as a summary on this topic ?  
  
From my day job and side-projects in the night, I usually see following scenarios when I need to suppress console output:  
  
* When a 3rd party pkg in your code produce unwanted console stdout, and I dont't want it to appear in the formatted output of your commmand line tool.
* I have two tools work together, one of the two produces some stdout from intermediate executions, and finally print out expected json as output. Then, for parsing simplicity for the second tool, I want the unecessary output apart from json to be suppressed.
* In my command line tool, I have a quiet mode option (usually appeared as `-q`, `-quiet`)

![suppress console output funny pic](/assets/img/blog-series-python/suppress-console-output-funny-pic.png)
>(image credit: https://hackernoon.com/terminal-thrillness-adventures-in-the-command-line-e3a1179883bd)  
  
Since this is a post within my blog series on python, lets look at the ways I found, works and would really like to recommend to you.  
  
Solutoin summary:  
> I have found different ways that work for different situations, but for most cases, the generic logic to suppress console output is temporarily overriding system stdout stream with a different stream.

**Lets look at a simple working solution:**  

```python
import sys

std_ref = sys.stdout
# open up the special null device & it discards everything written to it.
sys.stdout = open('/dev/null', 'w') 
######################
# Within current process, std console output from
# executoin in this area will be suppressed.
######################
sys.stdout = std_ref # resume original stdout stream.
```

This will work perfectly if you just need it in one place. However, it becomes less pretty and error-prone when you use it in multiple places. First, you need to repeat the stream swap operation everytime you need it, which is not pretty. Second, when you use it in many places, you are more likely to forget switching original stdout stream back to `sys.stdout`, which is error-prone and adds on debugging time.  
  
With that being said, **lets look at a pretty solution:**  

```python
import contextlib
import sys

@contextlib.contextmanager
def suppress_stdout(suppress=True):
    std_ref = sys.stdout
    if suppress:
        sys.stdout = open('/dev/null', 'w')
        yield
    sys.stdout = std_ref


with suppress_stdout():
    doAnything() # Console stdout will be suppressed.
```

So, with context manager, we save the hassile of repeatition on stream swap. In addition, it makes sure that original stdout stream is switched back everytime, which is the beauty of context manager.
  
For those who don't have a sense of python context manager, it is a way of managing resources in a less error-prone way. For example, usually when you open file with `open()`, you might forget to close it after you are done writing/reading with the file. One or two of such forgetness is fine, but when you have more and more, you are likely to reach the limit of the system you are on and cause issue. In addition, if we take a step back, any leakage should be avoided from a best practice point of view. Then, with context manager, you
can easily wrap the opening and closing in a `context` and apply it whenever you need it using `with`, which saves the hassile of remembering to close file stream after opening it.  
`open()` is a great example:  

```python
with open('/path/to/any/file', 'w') as f:
    # f is a handle referencing the file opened.
    # It is only valid in this context.
    # Read/write the file as you like through the handle.
    pass
```
  
without the decorator from `contextlib`, one more way to create a context manager is through definition of `__enter__` and `__exit__` function in a class:  
  
```python
# I am re-writing the console stdout suppressing function
# for a demonstration.
import sys

class SuppressStdout(object):
    
    def __init__(self, suppress=True):
        self.suppress = suppress
        self.sys_stdout_ref = None

    def __enter__(self):
        self.sys_stdout_ref = sys.stdout
        if self.suppress:
            sys.stdout = open('/dev/null', 'w')
        return sys.stdout

    def __exit__(self, type, value, traceback):
        sys.stdout = self.sys_stdout_ref


with SuppressStdout():
    doAnything() # do anything here.
```
  
Ok, lets come back to our focus on suppressing console stdout.  
  
So the second solution is much prettier and more practical. However, if we look at the stdout stream passed to `os.stdout`, it is on disk and there is still writing going on. Since we simply don't want any stdout whatsoever, can we get a stream with no writing ?  which will be more efficient as it saves the time of writing unneeded stdout.  
  
There is! We can simply construct an object with no-operation `write` function.  
  
```python
# To create a context manager, you can also 
# use contextlib.contextmanager decorator.
# I use second way as mentioned above here:

class NoOpStream(object):

    def write(self):
        pass

class SuppressStdout(object):
    
    def __init__(self, suppress=True):
        self.suppress = suppress
        self.sys_stdout_ref = None

    def __enter__(self):
        self.sys_stdout_ref = sys.stdout
        if self.suppress:
            sys.stdout = NoOpStream()
        return sys.stdout

    def __exit__(self, type, value, traceback):
        sys.stdout = self.sys_stdout_ref

with SuppressStdout():
    doAnything() # do anything here.
```
  
Now, from efficiency point of view, I don't think you will get anything cleaner, more efficient than this.  
  
However, we can compress the code above and make it more elegant looking:  

```python
class SuppressStdout(object):
    
    def __init__(self, suppress=True):
        self.suppress = suppress
        self.sys_stdout_ref = None

    def __enter__(self):
        self.sys_stdout_ref = sys.stdout
        if self.suppress:
            sys.stdout = self
        return sys.stdout

    def __exit__(self, type, value, traceback):
        sys.stdout = self.sys_stdout_ref

    def write(self):
        pass

with SuppressStdout():
    doAnything() # do anything here.
```
  
There is no need to separately maintain a class just for an op-operation `write` function, so why not melt it into `SuppressStdout` ? So finally, I present you this elegent solution as the conclusion for this post.  
