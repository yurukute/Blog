---
title: "Maximum subarray sum"
date: 2021-01-17T20:45:26+07:00
tags: ["Dynamic Programing"]
#ShowToc: true
TocOpen: true
---

## Problem:
Source: [Codewars.com](https://www.codewars.com/kata/54521e9ec8e60bc4de000d6c)

The maximum sum subarray problem consists in finding the maximum sum of a contiguous subsequence in an array or list of integers:
```
maxSequence({-2, 1, -3, 4, -1, 2, 1, -5, 4});
//should be 6: {4, -1, 2, 1}
```
Easy case is when the list is made up of only positive numbers and the maximum sum is the sum of the whole array. If the list is made up of only negative numbers, return 0 instead.

Empty list is considered to have zero greatest sum. Note that the empty list or array is also a valid sublist/subarray.
## Solution
### Approach 1: Brute Force
This is the most accessible way to  solve this promblem. It has 2 step:
1. Find **each** sum of each subarray {-2}, {1}, {-2, 1},...
2. Compare them to each other to find the largest

Beside its complexity is $O(n^3)$, however, this way has 3 weaknesses:
1. Bottlernecks
2. Unnessary works
3. Duplicate works

Therefore, we'll need a more efficient solution for this kata.
## Approach 2:
- We call $f[i]$ is a sum of $i$ first elements, we have $f[i]$ calculated:
    - $f[0] = 0$
    - $f[i] = f[i-1] + arr[i]$
- So, the sum from $i$th to $j$th element is $f[j] – f[i-1]$

With this, we can find $f[i]$ without duplicate works because we found $f[i-1]$ => We avoided the first approach's 3rd weakness (UwU). The complexity now is $O(n^2)$. But, this kata can also be solved with an $O(n)$ solution: Kadane's solution OwO)/
## Approach 3:
With second approach's idea, we use a variable `sum` to save the calculating sum. We'll have 2 choices to ask ourself while calculating: Do we add this number to `sum` **OR** we start a new subarray/new sum at this number? So that, we have to compare the [present `sum` plus `arr[i]`] with the `arr[i]` itself. In other words, we find `max(sum + a[i], a[i])` aka the result.
```cpp
#include <vector>

using namespace std;

int maxSequence(const vector<int>& arr){
    int max=0, sum=0, size=arr.size();
    for(int i=0; i<size; i++){
        sum+=arr[i];
        if(arr[i]>sum)
            sum=arr[i];
        if(sum>max) max=sum;
    }
    return max;
}
```
## Thanks for reading