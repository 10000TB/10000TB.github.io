---
layout: "post"
title: "A quick note on explaining a regex: raw string, grouping reference"
comments: true
publish: true
categories: regex
tags: [blogseries, regex, rawstring, groupingreference]
image: "blog-series-regex/blog-series-demystifying-regex-style1.png"
---

I was reading a thread regarding a solution to algorithm problem online, and there was one person asking what `r'\1-'` mean.  
  
There was one answer to it right down the question, but it didn't touch many details. So I shared my explanation to it in more detail. Since I am running a blog series on regex, so I thought it would make sense to move my explanation here to my blog under this series.  
  
The problem:  
  
  ```
  What is

  r’\1-’

  ?
  ```
  
My explanation:  
  
There are two syntax sugar here:
1. First, `r` at the beginning denotes that it is a raw string, which tells python not to interpret special sequence prefixed with backslash like: \w (match white space), \W(match non-whitespace), \d(match decimal digit, equivalent to 0-9), etc. Therefore, `r'\w'`, represent a regex that match a `backslash` followed by a `w` character.
2. Second, `\1` denotes a reference previously matched group. Simply, a group is a matched pattern coped with `()`. with `\1`, you reference it for future use. Pls do note that it has to be outside character class to be effective (it cannot be inside square brackets `[]`, which denotes a character class in regex).
3. `-`, is just matching a hyphen(-)
  
  
Reference(s):  
1. Here is the link to orignal thread I was reading: https://leetcode.com/problems/license-key-formatting/discuss/96511/Python-solution-based-on-regex/119946?page=1
