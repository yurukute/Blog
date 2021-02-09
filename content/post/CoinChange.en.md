---
title: "Fewest coins make change"
date: 2021-02-08T15:29:18+07:00
tags: ["Dynamic Programing"]
#ShowToc: true
TocOpen: true
mermaid: true
---
## Problem
Source: [Leetcode](https://leetcode.com/problems/coin-change/)

You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

Examples:
```cpp
Input:  coins = {1,2,5}, amount = 11
Output: 3 //2 đồng 5 và 1 đồng 1
```
```cpp
Input:  coins = {2}, amount = 3
Output: -1 
```
## Solution
### Approach 1: Brute force
By above example, *amount* = 11. Let's image 5 is the last used coin then the fewest coins we need will be the fewest coins to make change whatever is left plus 1. In other words,
$$coinChange(11) = coinChange(11 - 5) + 1$$
Meanwhile, the problem become ***fewest coins make change whatever is left***. This is a subproblem. Same as coin 1 and 2, those subproblem can be showed as a diagram like this:
{{<mermaid>}}
graph TD;
    A((11))-->B((10))
    A-->C((9))
    A-->D((6))
    B-->E((9))
    B-->F((8))
    B-->G((5))
    C-->H((8))
    C-->I((7))
    C-->J((4))
    D-->K((5))
    D-->L((4))
    D-->M((1))
{{</mermaid>}}
I just draw a part of the diagram but as you can see, if we consider those subproblems one by one, we will have to recalculate some similar subproblems (number 9, number 8,... are calculated twice). Thus, we need a better way, that is dynamic programing.
## Approach 2: Dynamic programing
By the idea above, this time i have an array like this, called `dp`
|0|1|2|3|4|5|6|7|8|9|10|11|
|-|-|-|-|-|-|-|-|-|-|-|-|-|
|||||||||||||

Each cell is a subproblem: ***Fewest coins make change the amount $i$***. So, the answer at $11^th$ cell is the answer that we're looking for this problem. And because it's calculated by the cells on its left, so we can reused the answer of the subprolem and avoid duplicating them.

To begin, i'll fill the array like this:
|0|1|2|3|4|5|6|7|8|9|10|11|
|-|-|-|-|-|-|-|-|-|-|-|-|-|
|0|12|12|12|12|12|12|12|12|12|12|12|

For each cell, i'll consider each coin whether it is greater than the amount at that cell. If not, which means we can use that coin, the value for that cell would be
$$dp[i] = min(dp[i-coin] + 1,dp[i])$$
If $i = 1$ then $dp[1] = min(0+1, 12)$. Just like that, at the end, the array will be filled as:
|0|1|2|3|4|5|6|7|8|9|10|11|
|-|-|-|-|-|-|-|-|-|-|-|-|-|
|0|1|1|2|2|1|2|2|3|3|2|3|

$11^th$ cell's value is 3, that is the answer for the example test. From there, i have a code like this:
```cpp
vector<int> dp(amount+1, amount+1);
dp[0] = 0;
for(int i = 1; i <= amount; i++)
    for(int coin : coins)
        if(coin <= i && dp[i - coin] + 1 < dp[i])
            dp[i] = dp[i - coin] + 1;
return dp[amount];
```
That's is the idea. But in reality, this code would recieve `wrong answer` instantly after submit, because of the careless when we read the problem. They said:
> If that amount of money cannot be made up by any combination of the coins, return `-1`.
So we need to edit `return` line for that case to get `passed` UwU.
```cpp
return (dp[amount] <= amount ? dp[amount] : -1);
```
Thank you for reading.
## Related topics
[Total ways to make change](post/CountChange.en.md)