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
  `{m,n}` - at least m times and at most n times.  
  
  Important notes:  
  1. `*` matching is greedy. It will match as many as it can until it runs into first mismatch; If later portion of the pattern doesn't match, it will back up and try again with a fewer repeatition.  
  2. `{1, }` equivalent to `+`.  
  3. When omitting upper bound, it will result in an upper bound of infinity.  
  4. When omitting lower bound, it will be interpreted as zero.  

- [x] Use raw string to represent regular expression.  
  To avoid [backslash plague](https://docs.python.org/2.7/howto/regex.html#the-backslash-plague), use raw string to represent regular expression for concise representation.  

- [x] Examples of using Regular expression.  
  One way is to first compile regular expression into pattern object, and perform match functions from the pattern object against target string.  

  ```python
  import re
  
  p = re.compile(r'^[a-z]+$')
  p.match("IamAnExample")
  ```

- [x] Functions available under pattern object for performaing matches.  
  After we compile regular ex, we can perform matching functins under the pattern:  
  `match()` - If RE matches the beginning of the string.  
  `search()` - search through string, and look for matches at any location.  
  `findall()` - find all matching substring(s), and return as a list.  
  `finditer()` - finall all matching substring(s), and return as a list.  
   
  ```python
  In [155]: test = "I am a doctor hu a ha, meaning I am a real"
  
  In [156]: import re
  
  In [157]: p = re.compile(r'[a]{1,1}')
  
  In [158]: p.findall(test)
  Out[158]: ['a', 'a', 'a', 'a', 'a', 'a', 'a', 'a']
  
  In [159]: it = p.finditer(test)
  
  In [160]: for match in it:
       ...:     print match.span()
       ...:
  (2, 3)
  (5, 6)
  (17, 18)
  (20, 21)
  (25, 26)
  (33, 34)
  (36, 37)
  (40, 41)
  
  In [161]:
  ```

- [x] Functions available under match object.  
  After we get a match object, we can then query info about match(es):  
  `group()` - return the string matched.  
  `start()` - starting position.  
  `end()` - ending position.  
  `span()` - return tuple containing (start, end).  

- [x] Compilation flags.  
  We can specify compilation flags to modify the effects of our regular exoression when we compile it.  

  ```python
  p = re.compile(r'JHadjad', re.IGNORECASE) # to ignore case when match.
  ```  

  note that multiple options are allowed by using `|` to join them.  
  Flags available:  
  [Flags list](https://docs.python.org/2.7/howto/regex.html#compilation-flags)

- [x] Advanced/More Metacharacters.  
  
  `|` - olternation, low precendence.  `adad|TFSS`, it will be interpreted as `adad` or `TFSS`.  
  `^` - match the beginning of lines. It will also match immediatelt after new line character in multiline mode when giving such compilation option during compilation time.  
  `$` - match at the end of a line -> either end of a string or any location followed by a newline character.  
  `\A` - start of a string.
  `\b` - word boundary.
  `\B` - opposite of `\b`.  

- [x] Grouping.  
  marked by `(` and `)`.  
  reteievd by `m.groups()`, `m.group(1)`( with group number as arg)


Reference:  
1. https://docs.python.org/2.7/howto/regex.html  
