---
layout: "post"
title: "[leetcode] straightforward python solution No-sorting - W/ thinking process and solution explanation"
comments: true
categories: code
tags: [leetcode, code, solution]
---

A micro post of a solution to a coding question - https://leetcode.com/problems/last-stone-weight/

**Intuition**: Reduce the problem to a sub-problem and recusrively solve it.

**Explanation**: Start by finding largest and second largest stone in the list, and destroy one of them or both based on their value, then we have reduced stones array by either 1 or 2 depends on if they are equal. Then we recursively call on the remaining stones. Base case is when stones array has one item in it, then we simply return it.

```python
def lastStoneWeight(self, stones):
    """
    :type stones: List[int]
    :rtype: int
    """
    if not stones: return 0
    if len(stones) == 1: return stones[0]
    x = y = None
    for i, s in enumerate(stones):
        if y is None or s > stones[y]: x = y; y = i
        elif x is None or s > stones[x]: x = i
    f, s = min(x, y), max(x, y)
    nxt = stones[:f] + stones[f+1:s] + stones[s+1:]
    if stones[f] == stones[s]:
        return self.lastStoneWeight(nxt)
    else:
        return self.lastStoneWeight(nxt+[stones[y]-stones[x]])

```
