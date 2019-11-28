---
layout: "post"
title: "[leetcode] - A soluton to \"Keyboard Row\""
categories: code
tags: [leetcode, code, soluton]
---

A micro post of a solution to a coding question - https://leetcode.com/problems/keyboard-row/description/

**Intuition**: Using set to compare if all letters of a word belong to a row.

**Details**: Save rows of characters into 3 different sets, and for each word we simply compare original word's set form with the intersection of it with any of the row. If there is a row's intersection are same as original one, then it qualifies. Small nuances: lower case the characters for comparison.

```python

def findWords(self, words):
    """
    :type words: List[str]
    :rtype: List[str]
    """
    r1 = {'q', 'w', 'e', 'r', 't', 'y', 'u', 'i', 'o', 'p'}
    r2 = {'a', 's', 'd', 'f', 'g', 'h', 'j', 'k', 'l'}
    r3 = {'z', 'x', 'c', 'v', 'b', 'n', 'm'}
    def check(w):
        w = w.lower()
        if not w: return False
        if w[0] in r1: return r1&set(w) == set(w)
        elif w[0] in r2: return r2&set(w) == set(w)
        elif w[0] in r3: return r3&set(w) == set(w)
        return False
    return filter(check, words)

```
