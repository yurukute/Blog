---
title: "Explosive Sum"
date: 2021-02-15T18:49:58+07:00
tags: ["Dynamic Programing"]
#ShowToc: true
TocOpen: true
---
## Problem
Source: [Codewars](https://www.codewars.com/kata/52ec24228a515e620b0005ef)

**How many ways can you make the sum of a number?**

From wikipedia: [https://en.wikipedia.org/wiki/Partition_(number_theory)#](https://en.wikipedia.org/wiki/Partition_(number_theory)#)
>In number theory and combinatorics, a partition of a positive integer n, also called an integer partition, is a way of writing n as a sum of positive integers. Two sums that differ only in the order of their summands are considered the same partition.
Example:
```cpp
Input:  4
Output: 5 //4, 3+1, 2+2, 2+1+1, 1+1+1+1
```
## Solution
It would be very slow if you enumerate all the partitions when $n$ is large so i'll use dynamic programing instead (Oω<)☆.

I have a dynamic table here (`ways`):
||0|1|2|3|4|
|-|-|-|-|-|-|
|0||||||
|1||||||
|2||||||
|3||||||
|4||||||

So, `ways[i][j]` is *Total ways to write j as a sum of positive numbers which $\leq i$*.

Partition of number $j$ can be divided into 2 types
- Not include $i$
 
    The problem now become *Total ways to write j as a sum of positive numbers which $< i$*, in other words, $\leq i - 1$
- Include $i$

    Then, if i remove $i$, i'll have ways to partition number $j - i$

Obviously, the partition can only be type #1 if $i > j$. In contrast, total ways is the sum of 2 types above. Therefore,
$$ways[i][j]=
    \begin{cases} 
        ways[i-1][j],                   & i > j  \\\\
        ways[i-1][j] + ways[i][j-i],    & i \leq j
    \end{cases}
$$

To calculate `ways[i][j]` i need to find  `ways[i-1][j]` and `ways[i][j-i]`, so at first, i'll fill the zero row. Because there is only one way to partition 0 so `ways[0][0] = 1`, others would be 0. After using recurrence relation to find all the cells, `ways[n][n]` would be the result that we need.
```cpp
#include<vector>

using ull = unsigned long long;

ull exp_sum(unsigned int n) {
    std::vector<std::vector<ull>> ways(n+1, std::vector<ull> (n+1, 0));
    ways[0][0] = 1;
    for(unsigned int i = 1; i <= n; i++){
        for(unsigned int j = i; j <= n; j++){
            ways[i][j] = ways[i-1][j];
            if(i <= j)
                ways[i][j] += ways[i][j-i];
        }
    }
    return ways[n][n];
}
```
||0|1|2|3|4|
|-|-|-|-|-|-|
|0|1|0|0|0|0|
|1|1|1|1|1|1|
|2|1|1|2|2|3|
|3|1|1|2|3|4|
|4|1|1|2|3|**5**|
### Improvement
As you can see, there is no need to store from row $1^{st}$ to $n^th-1$ because we only need row $n^{th}$ to calculate row $n^{th}+1$. That's why i'll use 1 dimmension array to store the values and calculate itself.
```cpp
#include<vector>

using ull = unsigned long long;

ull exp_sum(unsigned int n) {
    std::vector<ull> ways(n+1, 0);
    ways[0] = 1;
    for(unsigned int i = 1; i <= n; i++){
        for(unsigned int j = i; j <= n; j++){
            ways[j] += ways[j - i];
        }
    }
    return ways[n];
}
```
|0|1|2|3|4|
|-|-|-|-|-|
|1|1|2|3|**5**|

Thank you for reading.
## Related post
[Total ways to make change]({{< ref "post/CountChange">}})