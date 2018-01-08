---
layout: "post"
title: "Some arrogant clarification on a problem: Guess number higher or lower II"
comments: true
---

To ask people to solve a problem in order test his capability of problem solving, it is suggested, I believe, to describe to him/her the problem clearly.<br/>
<br/>
below is a problem I found online, and solved before. But I found original problem description is vague in a manner that the problem is unsolvable initially when reaching it just by its description. Therefore, I think it would be nice to share some clarification on what actually this problem is.<br/>
<br/>
The problem:<br/>

```
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number I picked is higher or lower.

However, when you guess a particular number x, and you guess wrong, you pay $x. You win the game when you guess the number I picked.

Example:

n = 10, I pick 8.

First round:  You guess 5, I tell you that it's higher. You pay $5.
Second round: You guess 7, I tell you that it's higher. You pay $7.
Third round:  You guess 9, I tell you that it's lower. You pay $9.

Game over. 8 is the number I picked.

You end up paying $5 + $7 + $9 = [1.
Given a particular n ≥ 1, find out how much money you need to have to guarantee a win.]
```

<br/>
(problem description credit: https://leetcode.com/problems/guess-number-higher-or-lower-ii/description/)

What I said about the problem description:<br/>
<br/>
It is actually confusing that the example shown in the problem description is not the best stragety to guess the final target number, and the problem itself is asking for the lowest cost achieved by best guessing strategy.<br/>
The example description should be updated.<br/>
<br/>
`---POSSIBLY, it can also add some example about the BEST Strategy---`
<br/>
The example description should be:<br/>
<br/>
First introducing the best strategy to guess:<br/>
<br/>
1. `For one number`, like 1, best strategy is 0$ <br/>
2. `for two numbers`, like 3,4, best strategy is 3$, which can be understood in this way: you have two way to guess: a) start by guess 4 is the target, (the worst case is) if wrong, you get charged $4 then immediately you know 3 is the target number, get get charged $0 by guessing that, and finally you get charged 4$. b) similarly, if you start by 3, (the worst case is) if wrong, you get charged $3, then you immediately know that 4 is the target number, and get charged $0 for guessing this, and finally you get charged $3. In summary:<br/>
range ---------> best strategy cost<br/>
3, 4 ---------> $3<br/>
5, 6 ---------> $5<br/>
...<br/>
<br/>
3. `for three numbers`, the best strategy is guess the middle number first, and (worst case is) if wrong, you get charged that middle number money, and then you immediately know what target number is by using "lower" or "higher" response, so in summary:<br/>
range ---------> best strategy cost<br/>
3, 4, 5 ---------> $4<br/>
7, 8, 9 ---------> $8<br/>
...<br/>
<br/>
4. `for more numbers`, it can simply be reduced them into smaller ranges, and here is why DP solution make more sense in solving this.<br/>
suppose the range is [start, end]<br/>
The strategy here is to iterate through all number possible and select it as the starting point, say for any k between start and end, the worst cost for this is: k+DP( start, k-1 ) + DP(k+1, end ), and the goal is minimize the cost, so you need the minimum one among all those k between start and end.<br/>
<br/>
<br/>


{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}
