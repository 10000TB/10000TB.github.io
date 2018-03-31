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

  Notes:  
  1. Identify group by its opening paranthesis since groups are numered from left to right.  
  2. group zero is by default for the full string matched.  
  3. groups() return all groups except group zero.  

- [x] Non-capturing and named groups.  
  Perl supports `(?...)` as ex extehsion syntac as it for sure wasn't being used by any old regular expression since it is considered a syntax error.  
  Python `re` module adds a `P` after question mark to denote that it is an extension specific to python.  
  For example:  
  `(?P<name>...)` represents a named group.  
  `(?P=name)` back reference to a named group.  
  
  **non-capturing group**  
  with `(?:...)` where `...` can be any other regular expression.  
  When we are interested in collecting part of a regular expression, but aren't interested in the contents of it.  
 
  ```python
  In [1]: import re
  
  In [2]: p = re.compile(r'[abc]+')
  
  In [3]: m = p.match("aaaaaac")
  
  In [4]: m.groups()
  Out[4]: ()
  
  In [5]: p = re.compile(r'([abc]+)')
  
  In [6]: m = p.match("aaaaaac")
  
  In [7]: m.groups()
  Out[7]: ('aaaaaac',)
  
  In [8]: m.group(0, 1)
  Out[8]: ('aaaaaac', 'aaaaaac')
  
  In [9]: p = re.compile(r'(?:[abc]+)')
  
  In [10]: m = p.match("aaaaaac")
  
  In [11]: m.groups()
  Out[11]: ()
  
  In [12]: m.group(0, 1)
  ---------------------------------------------------------------------------
  IndexError                                Traceback (most recent call last)
  <ipython-input-12-f65d9b02528f> in <module>()
  ----> 1 m.group(0, 1)
  
  IndexError: no such group
  
  In [13]: m.group(0)
  Out[13]: 'aaaaaac'
  
  In [14]:
  ```

  Notes:  
  1. There is no difference between a capturing grouping and non-capturing grouping except that non-capturing group does not return captured content.  
  2. There is no performance difference between a capturing group and non-capturing group.  
  3. With this we can add a new group without how existing groups are numbered.  

  **named group**  

  ```python
  In [21]: p = re.compile(r"(?P<alpha>[a-zA-Z]+)(?P<num>[0-9]+)(?P<other>[#$!!@#@]*)(?P=alpha)")
  
  In [22]: m = p.match("iamdavid01234!iam")
  
  In [23]: m
  
  In [24]: print m
  None
  
  In [25]: m = p.match("iamdavid01234!iamdavid")
  
  In [26]: print m
  <_sre.SRE_Match object at 0x1032a8880>
  
  In [27]: m.groups()
  Out[27]: ('iamdavid', '01234', '!')
  
  In [28]:
  ```
  
  The `(?P=name)` indicates that content matched by the group called `name` should also be matched at current location.  
 
- [x] Lookahead Assertions.  
  With lookahead assertions, we can tell match engine only move forward for further matching when assertion is successfull or failed.  
  
  `(?=...)` - Positive lookahead.  
  It will succeed if contained regular expression `...` match at the current location.  Matching engine will not continue moving further after that, but start matching again from current position. If assertion failed, the whole match will fail immediately. So the assertion here is really like an `if` condition to tell matching engine to move forward or not.  
  
  `(?!...)` - Negative lookahead.  
  
  For example, to match a file name, and exclude file suffix like `bat`, and `exe`, `csv`  e can write following expression:  
  ```python
  .*[.](?!bat$|exe$|csv$)[^.]*$
  ```
  So for this regular expression, when it matches til the `.`(dot), matching engine will only move forward when the suffix is not ant of the `bat`, `exe`, or `csv`.  
 
- [x] Modify strings.  
  split strings into a list. Split wherever there is a match.  
  `pattern.split(string[. maxsplit=0])`  

  search and replace(left-most non-overlapping ocurrences of RE).  
  `pattern.sub(replacement, string[, count=0])`  
  
  (`subn()` does the same thing, but return an extra count of total replacements together with first string in a tuple.)

  references: https://docs.python.org/2.7/howto/regex.html#search-and-replace


- [x] Common problems of regex.  
  reference: https://docs.python.org/2.7/howto/regex.html#common-problems
 

 
Reference:  
1. https://docs.python.org/2.7/howto/regex.html  
