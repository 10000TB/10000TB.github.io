---
layout: "post"
title: "[leetcode] - A solution to \"Check Majority Element in Sorted Array\" "
categories: code
tags: [leetcode, code, soluton]
---

A micro post of a solution to a coding problem - https://leetcode.com/problems/check-if-a-number-is-majority-element-in-a-sorted-array/description/

**Intuition:** binary search for the index of first and last occurence of the target, and then use them to 
get the count of target, compare it with the length of the original array for the answer.

Use built-in `bisect` to do binary search. `bisect.bisect_left(array, target)`, by implementation, returns left most index of target when there are more than one target. In comparison, `bisect.bisect_right(array, target)`, return index of the element right after right most occurrence of target.

**Example:**
Say there is any array `A = [....., 6,  7, 7, 7, 7, 7, 8, 9, 9 ....]`
, when we do `bisect_left` on `7`, it will return index of first 7 in the sequence. When we do `bisect_right`, it will return index of the element right after last occurrence of 7. In this case, it will be index of 8. Then with these two indexes, we get total number of 7s in the array.

```python
 def isMajorityElement(self, nums, target):
     """
     :type nums: List[int]
     :type target: int
     :rtype: bool
     """
     return len(nums)/2 < bisect.bisect_right(nums, target) - \ 
	     bisect.bisect_left(nums, target)
```
