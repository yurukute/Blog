---
title: "Longest increasing subsequence"
date: 2021-03-19T19:44:24+07:00
tags: ["Dynamic Programing", "Binary search"]
#ShowToc: true
TocOpen: true
---

## Problem 
Source: 
- Vietnamese:
    - [Codeforces - Easy](https://codeforces.com/group/FLVn1Sc504/contest/274501/problem/F)&nbsp;&nbsp;&nbsp;*Can be solve with $O(n^2)$ solution*
    - [Codeforces - Hard](https://codeforces.com/group/FLVn1Sc504/contest/274501/problem/G)&nbsp;*Need to be solved in $O(n\log n)$ solution*
- English:
    - [Leetcode](https://leetcode.com/problems/longest-increasing-subsequence/)

Give an integer array `nums`. Return the length of the longest increasing subsequence.

An increasing subsequence is a subsequence $a_1,..,a_k$ that
$$
\begin{align}
    &i_1 < i_2 < \dots < i_k,\\\\\\
    &nums[i_1] < nums[i_2] < \dots < nums[i_k]
\end{align}
$$
For example:
```cpp
Input:  {0,1,0,3,2,3}
Output: 4 //{0,1,2,3}
```
## Solution
### Brute Force
This method is the simplest solution can approach: We can enumerate all subsets of the original array and then test them for the increasing property then find the longest. This is too expensive with $O(2^n)$ complexity while $n$ can be a large number
### Dynamic programing $O(n^2)$
To solve this, I at first divide the problem into subproblems and find the answer for each. For *`nums` = {0,1,0,3,2,3}*, the subproblems I need to answer are:
***Find the length of the longest increasing subsequence:***\
    [0-0]&nbsp;&nbsp;&nbsp;*{0}*\
    [0-1]&nbsp;&nbsp;&nbsp;*{0, 1}*\
    [0-2]&nbsp;&nbsp;&nbsp;*{0, 1, 0}*\
    [0-3]&nbsp;&nbsp;&nbsp;*{0, 1, 0, 3}*\
    [0-4]&nbsp;&nbsp;&nbsp;*{0, 1, 0, 3, 2}*\
    [0-5]&nbsp;&nbsp;&nbsp;*{0, 1, 0, 3, 2, 3}* $\Rightarrow$ Original problem

So, my dynamic programing table `d` would have 5 cells, each cell store the answer of a subproblem. Assume that I've found the answer for cell `0`, `1`, `2` and I'm fiding the answer for cell `3`, then, I need to ask myself *3* questions:
- Can I extend $nums[3]$ into subsequence i've found in cell $d[0]$?
- Can I extend $nums[3]$ into subsequence i've found in cell $d[1]$?
- Can I extend $nums[3]$ into subsequence i've found in cell $d[2]$?

And if i can (that means $nums[3] > nums[i]$), `should` I extend it or keep the current answer at that cell.

Generally, $$d[i] = \max_{\substack{j = 0 \dots i-1 \\\\\\ nums[j] < nums[i]}} \left(d[j] + 1\right)$$

The automatic / default answer for `d` would be 1 because if the array has only 1 element, the array is the longest subsequence itself. My job is increasing that default value while processing.
|i|0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|$nums$|0|1|0|3|2|3|
|$d$|1|1|1|1|1|1|

Then, the fomular become $$d[i] = \max\left(1, \max_{\substack{j = 0 \dots i-1 \\\\\\ nums[j] < nums[i]}} \left(d[j] + 1\right)\right)$$

Finally, the answer is cell $d[i]$ that contain the largest value.
```cpp
int lis(vector<int>& nums){
    int n = nums.size(), max = 1;
    std::vector<int> lis(n,1);
    for(int i = 1; i < n; i++){
        for(int j = 0; j < i; j++)
            if(nums[j] < nums[i] && lis[i] < lis[j]+1)
                lis[i] = lis[j] + 1;
        if(lis[i] > max)
            max = lis[i];
        }
    return max;
}
```
|i|0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|$nums$|0|1|0|3|2|3|
|$d$|1|2|1|3|3|**4** $\Rightarrow$ result|
### DP and Binary search $O(n\log n)$
I keep the dynamic programing table `d` but to store the element at which a subsequence of length $i$ terminates.

Initially we assume $d[0] = -\infty$ and for all other elements $d[i] = \infty$.\
the length of the desired subsequence is the largest $l$ with $d[l] < \infty$.
```cpp
int lis(vector<int>& nums) {
    int n = nums.size(), max = 1;
    const int INF = 1e9;
    std::vector<int> d(n+1, INF);
    d[0] = -INF;
    for (int i = 0; i < n; i++)
        for (int j = 1; j <= n; j++) {
            if (d[j-1] < nums[i] && nums[i] < d[j])
                d[j] = nums[i];
        }
    for (int i = 0; i <= n; i++)
        if (d[i] < INF) max = i;
    return max;
}
```
|i|0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|$nums$|0|1|0|3|2|3|
|$d$|$-\infty$|0|1|2|3|$\infty$|

I noticed that:
- `d` is always sorted
- $nums[i]$ would update at most one value $d[j]$

Thus, I can find this element in the `d` using *Binary search* in O(logn). In fact I'm simply looking in `d` for the first number that is strictly greater than $a[i]$, and trying to update this element in the same way as the above implementation.
```cpp
int lis(std::vector<int>& nums) {
    int n = nums.size(), max = 1;
    const int INF = 1e9;
    std::vector<int> d(n+1, INF);
    d[0] = -INF;
    for (int i = 0; i < n; i++){
        int j = upper_bound(d.begin(), d.end(), a[i]) - d.begin();
        if (d[j-1] < nums[i] && nums[i] < d[j])
            d[j] = nums[i];
    }
    for (int i = 0; i <= n; i++)
        if (d[i] < INF) max = i;
    return max;
```
Thank you for reading.