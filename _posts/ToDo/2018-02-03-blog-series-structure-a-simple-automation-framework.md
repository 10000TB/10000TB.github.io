---
layout: "post"
title: "Structure a simpe automation framework (1)"
comments: true
tags: [blogseries, automation, learn&share]
categories: automation
image: "blog-series-automation/blog-series-automation-banner.png"
---

> #automation_framework is likely to be more complex than what I write here given more sophisticated
needs. I would only speak for what I know, what I learnt, and hope to have this writing serve as a way
of practicing communicating my technical learnings and retrospectives.
  
I have been at my current work for testing automation for a year now, and I have seen many ways of building
base structures for automating test scenarios. With a mindset of learning and writing, I feel an intuitive
push from my heart that I should write a summary of the framework I have been dealing/working with, and test out If
I am really understanding how and why the framework was written by documenting/writing it down in an abstract way.
  
In an abstract way, the folder structure of a python based framework could be like follow:  
```
.
+-- test_case_base.py
+
+-- test_case_testbed.py
+
+-- karma/


```
