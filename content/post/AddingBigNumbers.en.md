---
title: "Adding Big Numbers"
date: 2022-06-07T20:43:38+07:00
tags: []
#ShowToc: true
TocOpen: true
---
# Problem
Source: [Codewars](https://www.codewars.com/kata/525f4206b73515bffb000b21)

We need to sum big numbers and we require your help.

Write a function that returns the sum of two numbers. The input numbers are strings and the function must return a string.

Example
```cpp
add("123", "321"); -> "444"
add("11", "99");   -> "110"
```
Notes
- The input numbers are big.
- The input is a string of only digits
- The numbers are positives
## Solution
In arithmetic, addition can be performed by aligning the addends vertically and add the colums, starting from the unit colum (on the right).
$$
\\begin{aligned}
    105 \\\\
    +\\quad6\\\\
    \\hline
     111\\\\
\\end{aligned}
$$
To simulate the steps, traverse two adden-strings from the last position, add their digits then append the sum to the result string. Because of the length difference and right alignment, the position is `length - i` for `i` in range `1` to `length`


Initialize `carry = 0` to store the extra digit if a column exceeds `9`, this is then transferred to the next column or appended into the result string after calculated all the colums.
```cpp
#include <string>

std::string add(const std::string& a, const std::string& b) {
    std::string res = "";
    int len_a = a.length(), len_b = b.length(), n = std::max(len_a, len_b);
    int carry = 0;

    for(int i = 1; i <= n; i++){
        if(i <= lenA)
            carry += a[lenA - i] - '0';
        if(i <= lenB)
            carry += b[lenB - i] - '0';
        res.insert(res.begin(), carry % 10 + '0');
        carry /= 10;
    }
    res.insert(res.begin(), carry + '0');

    while(res.length() > 1 && res.front() == '0')
        res.erase(res.begin());
    return res;
}
```
Thank you for reading.
