---
layout: "post"
title: "[leetcode] - A solution to \"Relative Sort Array\" "
categories: code
tags: [leetcode, code, soluton]
---

A micro post of a solution to a coding problem - https://leetcode.com/problems/relative-sort-array/description/

**Intuition**: Sort with custom comparison function.

**Details**: python 2 `sorted` comes with a argument `cmp` and it takes a callable where we can define a custom comparision function. It takes two arguments which drvies from original list when python executes it. Then we implement it based on problem statement.

We record relative position of elements from `arr2` in a key value mapping. Then simply comparing the index of each value we know which comes first. There are 3 simple scenarios we need to consider: 1) Both elements are from `arr2`, then we simply use their relative positions for comparison. 2) Both elements are not in `arr2`, then we simply use their literal value for comparison. 3) One of the two elements is from `arr2`, while the other are not, then we simply return a random position or negative value based which one is from `arr2`, to make sure the one from `arr2` comes before the one not.

```python
def relativeSortArray(self, arr1, arr2):
    """
    :type arr1: List[int]
    :type arr2: List[int]
    :rtype: List[int]
    """
    mp = {v:idx for idx, v in enumerate(arr2)}
    def rcmp(x, y):
        if x in mp and y in mp: return mp[x] - mp[y]
        elif x not in mp and y not in mp: return x - y
        else: return 1 if x not in mp else - 1
    return list(sorted(arr1, cmp=rcmp))
```
