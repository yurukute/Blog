---
title: "Total ways make change"
date: 2021-02-09T13:12:29+07:00
tags: ["Dynamic Programing"]
#ShowToc: true
TocOpen: true
---
## Problem 
Source: [Leetcode](https://leetcode.com/problems/coin-change-2/),
        [Codewars](https://www.codewars.com/kata/541af676b589989aed0009e7)

You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.

For example:
```cpp
Input:  amount = 5, coins = {1,2,5}
Output: 4 //{5, 2+2+1, 2+1+1+1, 1+1+1+1+1}
```
## Solution
We'll go through at each stage to considera certain coin and see how it changes the total amount of ways that can make change. Look at the dynamic programing table (`ways`) now:
| |0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|{}|||||||
|{**1**}|||||||
|{1,**2**}|||||||
|{1,2,**5**}|||||||

Each cell on the table is a subproblem and they are all related to eachother. Let's imagine i'm at the cell $ways[1][1]$, then i'll have to find the answer for *total ways make change* amount $1$ with no coin; if i'm at the cell $ways[1][1]$, i'll have to find the answer for *total ways make change* amount $3$ with coins {1,2}. So does value at cell $ways[i][j]$ would be the answer for *total ways make change* amount $j$ with coins at row $i$.

And notice, every row represents the addition of another coin (shown in bold). Thus, row $i$ will answer the question: *Will i use the $i^th$ coin?*

Obviously, i won't use the $i^th$ coin if $coins[i-1] > j$, on the contracy, i can. Therefore:
$$ways[i][j]=
    \begin{cases} 
        ways[i-1][j],                         & coins[i-1] > j \\\\
        ways[i-1][j] + ways[i][j-coins[i-1]],   & coins[i-1] <= j 
    \end{cases}
$$
Let me explain a bit about the formula above. When i don't use the coin, the subproblem become the same with the subproblem at the previous row, the row that don't contain $coins[i-1]$ then i'll go back that cell and use its value. But if i use the coin, the amount we have is only $j - coins[i-1]$ so i'll comeback the cell $ways[i][j-coins[i-1]]$ to get its value plus the case *[don't use that coin]*

## Fill the base
To begin, i have to fill the zero row and zero column. Zero row means we don't have anycoin so we can't make change, the value here would be `0`. The zero column means we use the coins to make change... zero amount, so that those cell's value would be `1` because the only way is **do nothing**.

For convenience, i'll create a 2 dimmension vector with all value are `0`. Then, i just have to fill the zero column.
```cpp
vector<vector<int>> ways(coins.size() +1, vector<int> (amount + 1, 0));
for(int i = 0; i <= coins.size(); i++)
    ways[i][0] = 1;
```
| |0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|{}|1|0|0|0|0|0|
|{**1**}|1|0|0|0|0|0|
|{1,**2**}|1|0|0|0|0|0|
|{1,2,**5**}|1|0|0|0|0|0|

The next thing to do is fill the remain cells. The final cell is the answer for this problem.
```cpp
for(int i = 1; i <= coins.size(); i++)
    for(int j = 1; j <= amount; j++)
        ways[i][j] = ways[i-1][j];
        if(j >= coins[i-1])
            ways[i][j] += ways[i][j - coins[i-1]];
return ways[coins.size()][amount];
```
| |0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|{}|1|0|0|0|0|0|
|{**1**}|1|1|1|1|1|1|
|{1,**2**}|1|1|2|2|3|3|
|{1,2,**5**}|1|1|2|2|3|**4**|
### Improvement
The solution above has a weakness. After we calculated the $n^th$ row, we don't need to store the value from the $1^st$ row to the $(n-1)^th$ row because we only need $n^th$ to calculated the next row. Therefore, i'll change from 2 dimmensions array to 1 dimmension to store and calculate itself.
```cpp
vector<int> ways(amount + 1);
    ways[0] = 1;
    for(auto coin : coins)
        for(int i = 1; i <= amount; i++)
            if(i >= coin)
                ways[i] += ways[i - coin];
return ways[amount];
```
|0|1|2|3|4|5|
|-|-|-|-|-|-|
|1|1|2|2|3|**4**|

Thank you for reading.