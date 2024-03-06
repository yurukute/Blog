---
title: "Snail"
date: 2024-03-05T20:26:11Z
tags: ["Array"]
#ShowToc: true
TocOpen: true
---

## Problem
Source: [Codewars](https://www.codewars.com/kata/521c2db8ddc89b9b7a0000c1)

Given an n x n array, return the array elements arranged from outermost elements to the middle element, traveling clockwise.
```
array = [[1,2,3],
         [4,5,6],
         [7,8,9]]
snail(array) #=> [1,2,3,6,9,8,7,4,5]
```
For better understanding, please follow the numbers of the next array consecutively:
```
array = [[1,2,3],
         [8,9,4],
         [7,6,5]]
snail(array) #=> [1,2,3,4,5,6,7,8,9]
```
This image will illustrate things more clearly:

{{< svg "static/snail.svg" >}}
 
NOTE: The idea is not sort the elements from the lowest value to the highest; the idea is to traverse the 2-d array in a clockwise snailshell pattern.

NOTE 2: The 0x0 (empty matrix) is represented as en empty array inside an array [[]].

## Solution
This problem is simpler than its 4-kyuu label. To traverse in a snailshell pattern, I will traverse the 0th row, n-1th collumn, n-1th row, 1st collumn,.. respectively.

First of all is to declare these variables:
```cpp
std::vector<int> res;
int row_start = 0;                  // Top-most row
int col_start = 0;                  // Left-most collumn
int row_end = snail_map.size();     // Bottom-most row
int col_end = row_end;              // Right-most collumn
```
Start from the topmost row, increase the collumn index to traverse through it:
```cpp
for (int i = col_start; i < col_end; i++)  
    res.push_back(snail_map[row_start][i]);
```
The same with the rightmost collumn, increase the row index to traverse. It should be noted that the top right element (`snail_map[row_start][col_end-1]`) has to be skipped since it has been read before.
```cpp
for (int i = ++row_start; i < row_end; i++) 
    res.push_back(snail_map[i][col_end-1]);
```
Due to snailshell pattern, traversing the bottom-most row need to be done in backwards: decrease the collumn index instead of increasing. And because the bottom right element (`snail_map[row_end-1][col_end-1]`) has been read, the right most collumn will be skipped.
```cpp
for (i = (--col_end)-1; i >= col_start; --i) 
    res.push_back(snail_map[row_end-1][i]); 
```
Similarly, decrease the row index to traverse the left most collumn in backwards.
```cpp
for (int i = (--row_end)-1; i >= row_start; --i)
    res.push_back(snail_map[i][col_start]);
```

These operations will be looped until the given map is traversed, which means, `row_start` and `row_end`, or `col_start` and `col_end`, passed each others (`_start` $\geq$ `_end`). Besides, an empty map case should be considered.
```cpp
if(snail_map.empty() || snail_map[0].empty())
    return {};
```
With all that, the solution for this kata is completed.
```cpp
#include<vector>

std::vector<int> snail(const std::vector<std::vector<int>> &snail_map){
    if(snail_map.empty() || snail_map[0].empty())
        return {};

    std::vector<int> res;
    int row_start = 0;                  // Top-most row
    int col_start = 0;                  // Left-most collumn
    int row_end = snail_map.size();     // Bottom-most row
    int col_end = row_end;              // Right-most collumn

    while (row_start <= row_end && col_start <= col_end){  
        for (int i = col_start; i <= col_end; i++)  
            res.push_back(snail_map[row_start][i]); 
       
        for (int i = ++row_start; i <= row_end; i++) 
            res.push_back(snail_map[i][col_end]); 
        
        for (int i = --col_end; i >= col_start; i--)
            res.push_back(snail_map[row_end][i]); 
        
        for (int i = row_end; i >= row_start; i--)
            res.push_back(snail_map[i][col_start]);
        
        col_start++;
    }
    return res;
};
```
## Also a solution
If you are more familiar with `while` loop, here is how the solution could be rewritten:
```cpp
    int row = 0, col = 0;
    while (row_start <= row_end && col_start <= col_end) {
        while (col < col_end)   res.push_back(snail_map[row][col++]);  row_start++;
        while (row < row_end)   res.push_back(snail_map[row++][col]);  col_end--;
        while (col > col_start) res.push_back(snail_map[row][col--]);  row_end--;
        while (row > row_start) res.push_back(snail_map[row--][col]);  col_start++;
    }
    res.push_back(snail_map[row][col]); // Adding the last element
```
Two variables `row` and `col` are used to mark the current row and collumn, the two variables only change from `0` to `n-2`. so that the next `while` could make use of current values of `row` and `col` without traverse through the same element twice, in other words, instead of skipping the read element, I will leave the last element of the current row/col. And because of this, it is a need to add the last element of the map after the `while` loop is done.

Thank you for reading.