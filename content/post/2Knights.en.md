---
title: "Two Knights"
date: 2021-05-27T20:37:44+07:00
tags: ["Maths"]
#ShowToc: true
TocOpen: true
---
## Problem
Source: [CSES](https://cses.fi/problemset/task/1072)

Your task is to count for $k=1,2,…,n$ the number of ways two knights can be placed on a $k \times k$ chessboard so that they do not attack each other.

For example:
```
Input: 8
Output: 0
        6
        28
        96
        252
        550
        1056
        1848
``` 
## Solution
### Idea
The most reachable method is for each of the first knight's position, i'm looking for pertinent positions to place the second knight and sum them all. It's kinda reckless to create nested loops so i'll need a smarter way 👀

Instead of finding the answer directly, I can indirectly find it by subtract the `number of ways two knights can be placed to attack each other` from `total ways to place two knights`.

The `total ways to place two knights` is:
$$C^2_{k^2} = \frac{k^2!}{2!(k^2-2)!} = \frac{k^2(k^2-1)(k^2-2)!}{2(k^2-2)!} = \frac{k^2(k^2-1)}{2}$$
Noticed that, the knights can attack each other when they form a  diagonal line of a $2 \times 3$ (horizontal) or $3 \times 2$ (vertical) rectangle. A chessboard is a square board so the number of $2 \times 3$ are the same as $3 \times 2$, so that i just have to count one kind of those two and duplicate the result. 

Here are the steps:
- Start from top left, imagine i'm having a $2 \times 3$ rectangle there.
- Shift 1 square to right to find how many times i can shift it to the right. Because i'm not allowed to let 2 columns of the rectangle move out of the chessboard so i can shift it $n-2$ times.
- Similarly, shifting it to bottom and finding how many times i can shift it. There are $n-1$ times since i can't let the last row move out of the chessboard.
- Finally, multiply them and duplicate the answer.
$$2(n-1)(n-2)$$

Moreover, there are two diagonal lines in a rectangle, that means there are two ways the knights can attack each other in each case. Thus, `number of ways two knights can be placed to attack each other` is:
$$4(n-1)(n-2)$$

In the end, the result of this problem is:
$$\frac{k^2(k^2-1)}{2} - 4(n-1)(n-2)$$

### Code
```cpp
#include<iostream>

int main(){
	int n;
	std::cin >> n;
	for(long long i = 1; i <= n; i++){
		std::cout << i*i*(i*i - 1)/2 - 4*(i-1)*(i-2) << "\n";
	}
}
```
Thank you for reading 👀
