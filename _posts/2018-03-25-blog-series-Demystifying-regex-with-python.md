---
layout: "post"
title: "Demystifying regex with python"
published: true
categories: python
tags: [blogseries, python, generator]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy-style3.png"
---
**Preface**  
When you to extract a valid IP from a string, or when you have a string with a know pattern and want to separate it into a list, or when you have a string and you simply want to check if it falls into a pattern, how would you approach ?  
  
I bet there are tons of creative solutions out there for approaching each problem as mentioned above, but here I would like to share my thoughts and exploration on the beauty of using `re` regular expression module in python in solving problem as such.
  
**Introduction**  
It is said that regular expression is also a language, but relatively small and restricited.  
  
I have linked the official documentation on regular expression from python.org, and there are exhausted details there. This post, instead, will summarize regular expression in bullet points and leave pointers randomly to official doc where there are exhausted details on everything.  

![Regular expression logo - Fedexpress style](/assets/img/blog-series-python/regular-expression-fedex-funny.jpeg)
 
**1. Learn basic matching with `metacharacters`**  
To summarize regular expression, one should first introduce `metacharacters` and how to do simple matching. From there, more syntax introductions and advanced matching techniques come in naturally.  
  
- [x] `Metacharacters` are special characters used in regular expression or special purposes.
  A complete list of metacharacters and what they do in brief are as follow:  

  - `[` and `]` - open and enclosing square brackets - They are used to specify a `character class`, which is a way to specify what character(s) to match in regular expression.  
      * Characters can be listed individually or by range with `-`: `[abcs]`, `[a-c]`  
      * __Metacharacters are not active inside `[]`.__  
      * Reverse matching with `^`: putting `^` as first character inside a character class will match non-matching character(s): `[^5]`(match any non-5 character), `[^abc]`(match any character except a, b, and c)  
      * 
  - `^` - for now, lets say it does reversing matching when placed inside `[]` as first character. In addition, when placed outside character class, and usually beginning of a regular expression, it matches start of a line.  
  - `\` - It can be used to escape metacharacter so that can match metacharacter itself. In addition, some special sequences beginning with `\` represent predefined set of characters:`\d`, `\D`, `\s`, `\S`, `\w`, `\W`.  
  - `.` - Dot - It matches anything except a newline character. 

- [x] Repeating.  
  With character class, you can now match any character. But that is not enough to solve practical problems as there are scenarios where we need to match repeatitions of characters. Then how to specify matching pattern need to be resolved. In regular expression, ways to specify repeatitions are:  
  `*` - zero or more.
  `+` - one or more.
  `?` - zero or one.
  ``




Reference:  
1. https://docs.python.org/2.7/howto/regex.html  
