---
title: "Sum of intervals"
date: 2021-07-27T09:40:22+07:00
tags: ["Numbers"]
#ShowToc: true
TocOpen: true
---

## Problem
Source: [Codewars](https://www.codewars.com/kata/52b7ed099cdc285c300001cd)

Write a function called `sumIntervals`/`sum_intervals()` that accepts an array of intervals, and returns the sum of all the interval lengths. Overlapping intervals should only be counted once.

### Intervals
Intervals are represented by a pair of integers in the form of an array. The first value of the interval will always be less than the second value. Interval example: [1, 5] is an interval from 1 to 5. The length of this interval is 4.

### Overlapping Intervals
List containing overlapping intervals:
```
[
   [1,4],
   [7, 10],
   [3, 5]
]
```
The sum of the lengths of these intervals is 7. Since [1, 4] and [3, 5] overlap, we can treat the interval as [1, 5], which has a length of 4.

For examples:
```cpp
sum_intervals( {
   {1,2},
   {6, 10},
   {11, 15}
} ); // => 9

sum_intervals( {
   {1,4},
   {7, 10},
   {3, 5}
} ); // => 7

sum_intervals( {
   {1,5},
   {10, 20},
   {1, 6},
   {16, 19},
   {5, 11}
} ); // => 19
```
## Solution
Notice that, if an interval's left point is **smaller** than its preceding's right point, they're overlapping. There're two cases:
- This interval's head is another's tail. For example: [1, 4] & [3, 5]
    - The second interval's right point is **greater** than the first one's
    - The merged interval: [1, 5]
- This interval is included by another. For example: [1, 5] & [2, 4]
    - The second interval's right point is **smaller** than the first one's
    - The merged interval is the first one: [1, 5]

First, i need to sort `intervals` ascending by left points for not missing any overlapping interval which leads to an unexpected result. Using `merged` array to save the merged intervals, `merged.back()` serves as the right point of the considering interval's preceding. If the considering interval is overlapping with its preceding, update the preceding's right point - `merged.back()` - accordingly. Otherwise, add the considering interval into `merged`. 
```cpp
#include <vector>
#include <utility>
#include <algorithm>

int sum_intervals(std::vector<std::pair<int, int>> intervals) {
    int size = intervals.size();
    std::sort(intervals.begin(), intervals.end());
    std::vector<std::pair<int,int>> merged = {{intervals[0].first, inttervals[0].second}};
    for(int i = 1; i < size; i++){
        if(intervals[i].first <= merged.back().second)      //Overlapping
            if (intervals[i].second >= merged.back().second)    // Case (1)
                merged.back().second = intervals.second; 
            else continue;                                      // Case (2)
        else merged.push_back(intervals[i]);                // Not overlapping
    }
    int res = 0;
    for(const auto &i : merged)
        res += i.second - i.first;   
    return res;
}

```
### Improving 
Call `x` the right point of considering interval's preceding (means it has `merged.back().second`'s value). Still with two cases but change the order: 
- If the considering interval's right point is greater than or equal to x:
    - If its left point is **smaller** than x, they're overlapping in case (1)
    - If its left point is **greater** than x, they're not overlapping
    - Either they're overlapping or not, `x`'s value will need to be updated to the considering's right point
- Otherwise, if it's **smaller** than `x`, they're overlapping in case (2). Which means i can skip.

In this method i only need an `if` and not using `merged` made the code looks more simple.
```cpp
#include <vector>
#include <utility>
#include <algorithm>

int sum_intervals(std::vector<std::pair<int, int>> intervals) {
    std::sort(intervals.begin(), intervals.end());
    int x = intervals[0].first, res = 0;
    for (auto &i : intervals){
        if (i.second >= x){
            res += i.second - (i.first > x ? i.first : x);
            x = i.second;
        }    
    }
    return res;
}
```
Thank you for reading.