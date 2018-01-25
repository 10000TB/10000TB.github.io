---
layout: "post"
title: "Suppressing python console stdout with context manager: from working solution to elegant solution"
comments: true
categories: python
tags: [blogseries, python, contextmanager, suppressconsoleout]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---

There have been more than once, as I can recall, when I need to suppress console out to achieve one goal or two, and it then becomes natural from my blogging thinking: why don't I compile a post as a summary on this topic ?  
  
From my day job and side-projects in the night, I usually see following scenarios when we need to suppress console output:  
  
* When a 3rd party pkg in your code produce unwanted console stdout, and you dont't want it to appear in the formatted output of your commmand line tool.
* You have two tools work together, one of the two produces some stdout from intermediate executions, and finally print out expected json as outpur. Then, for parsing simplicity for the second tool, you want the unecessary output apart from json be suppressed.
* In your command line tool, you have a quiet mode option (usually appeared as `-q`, `-quiet`)

![suppress console output funny pic](/assets/img/blog-series-python/suppress-console-output-funny-pic.png)
>(image credit: https://hackernoon.com/terminal-thrillness-adventures-in-the-command-line-e3a1179883bd)  
  
Since this is a post within python, lets look at the ways I found, works and would really like to recommend to you.  
  
Solutoin summary:  
> I have found different ways that work for different situations, but for most cases, the generic logic to suppress console out is temporarily overriding system stdout stream with a different stream.

**Lets look a simple working solution:**  

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

This will work perfectly if you just need it in one place. However, it becomes less pretty and error-prone when you use it in multiple places. First, you need to repeat the stream swap operation everytime you need it, which is not pretty. Second, when you use it in many places, you are more likely to miss switching original stdout stream back to `sys.stdout`, which is error-prone and adds on debugging time.  
  
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
  
For those who don't have a sense of python context manager, it is a way of managing resources in a less error-prone way. For example, usually when you open file with `open()`, you might forget to close it after you are doen with the file. One or two of such forgettness is fine, but when you have more and more, you are likely to reach the limit of the system and cause issue. In addition, any leakage should be avoided from a best practice point of view. Then, with context manager, you
can easily wrap the opening and closing in a `context` and apply it whenever you need it, which save the hassile of remembering to close file stream after opening it.  
`open()` is exact example:  

```python
with open('/path/to/any/file', 'w') as f:
    # f is the handle of the file opened.
    # it is only valid in this context.
    # do things you like with the handle.
    pass
```
  
without the decorator from `contextlib`, one more way to create a context manager is through definition of `__enter__` and `__exit__` function in a class:  
  
```python
# Re-write the console stdout suppressing function:
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
  
So the second solution is much prettier and more pract
