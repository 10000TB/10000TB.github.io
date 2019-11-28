---
layout: "post"
title: "Question the question - \"Guess Number Higher or Lower II\" "
categories: code
tags: [leetcode, code, soluton]
---

I was working on a leetcode problem - https://leetcode.com/problems/guess-number-higher-or-lower-ii/description/

The problem itself fail to ask the right thing, which confuses a lot of problem solving the problem at that time. I compiled a post to further explain what is actually being aksed in the problem.

(Also shared here - https://leetcode.com/problems/guess-number-higher-or-lower-ii/discuss/84766/clarification-on-the-problem-description-problem-description-need-to-be-updated/248759)

It is actually confusing that the example shown in the problem description is not the best stragety to guess the final target number, and the problem itself is asking for the lowest cost achieved by best guessing strategy.
The example description should be updated.


```---POSSIBLY, it can also add some example about the BEST Strategy---```

The example description should be:

first introducebest strategyto guess:

1. ```for one number```, like 1, best strategy is 0$
2. ```for two number```, like 3,4, best strategy is 3$, which can be understood in this way: you have two way to guess: a) start by guess 4 is the target, (the worst case is) if wrong, you get charged $4, then immediately you know 3 is the target number, get get charged $0 by guessing that, and finally you get charged $4. b) similarly, if you start by 3, (the worst case is) if wrong, you get charged $3, then you immediately know that 4 is the target number, and get charged $0 for guessing this, and finally you get charged $3. In summary:
range ---------> best strategy cost
3, 4 ---------> $3
5, 6 ---------> $5
...
3. ```for three number```, the best strategy is guess the middle number first, and (worst case is) if wrong, you get charged that middle number money, and then you immediately know what target number is by using "lower" or "higher" response, so in summary:
range ---------> best strategy cost
3, 4, 5 ---------> $4
7, 8, 9 ---------> $8
...
4. ```for more numbers```, it can simply be reduced them into smaller ranges, and here is why DP solution make more sense in solving this.
suppose the range is [start, end]
the strategy here is to iterate through all number possible and select it as the starting point, say for any k between start and end, the worst cost for this is: k+DP( start, k-1 ) + DP(k+1, end ), and the goal is minimize the cost, so you need the minimum one among all those k between start and end
