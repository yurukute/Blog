---
title: "Roman Numerals Helper"
date: 2021-07-25T18:51:10+07:00
tags: []
#ShowToc: true
TocOpen: true
---
## The problem
Source: [Codewars](https://www.codewars.com/kata/51b66044bce5799a7f000003)

Create a RomanNumerals class that can convert a roman numeral to and from an integer value. 
```cpp
#include <string>
#include <vector>

class RomanHelper{
public:
    std::string to_roman(unsigned int n){

    }
    
    int from_roman(std::string rn){

    }
} RomanNumerals;
```
It should follow the API demonstrated in the examples below. Multiple roman numeral values will be tested for each helper method.

Modern Roman numerals are written by expressing each digit separately starting with the left most digit and skipping any digit with a value of zero. In Roman numerals 1990 is rendered: 1000=M, 900=CM, 90=XC; resulting in MCMXC. 2008 is written as 2000=MM, 8=VIII; or MMVIII. 1666 uses each Roman symbol in descending order: MDCLXVI.

For example:
```
RomanNumerals.toRoman(1000); // should return 'M'
RomanNumerals.fromRoman('M'); // should return 1000
```
Help
|Symbol |I  |V  |X  |L  |C  |D  |M   |
|:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|
|Value  |1  |5  |10 |50 |100|500|1000|

## Solution
### Convert integer to Roman Numeral 
Idea is to convert the thousands, hundreds, tens, units places of the given number separately. Suppose the number is greater than or equal to 1000, i will subtract 1000 from it and append the symbol `M` to the result string. Repeat untils it's smaller than 1000 then move to hundreds place. There are total 13 cases since some numbers can be written exceptionally like 4 (IV) and 9 (IX)
```cpp
std::string to_roman(unsigned int n){
    std::string res = "";
    std::string rnums[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
    int nums[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1}; 
    for(int  i = 0; i < 13; i ++){
        while (n >= nums[i]){
            res.append(rnums[i]);
            n  -= nums[i];
        }
    }
    return res;
}
```
### Convert Roman Numeral to integer
As we all know, each Roman symbol represents an integer. Thus, first of all, i covert the symbols to their integer value one by one.
```cpp
int from_roman(std::string rn){
    std::vector<int> a;
    for(char i : rn){
        if (i == 'I') a.push_back(1);
        if (i == 'V') a.push_back(5);
        if (i == 'X') a.push_back(10);
        if (i == 'L') a.push_back(50);
        if (i == 'C') a.push_back(100);
        if (i == 'D') a.push_back(500);
        if (i == 'M') a.push_back(1000);
    }
    ...
}
```
The purpose of this is to facilitate the comparison. Noticed that if one symbol is greater than or equal to its following, the value is their combination, for example: VI = V + I = 6. Otherwise, if that one symbol is smaller, just subtract it. I will compare each pair of continous numbers in `a` and do the junior math above to find the final result. Because there is no value to compare with the last element so it will be added first.
```cpp
int from_roman(std::string rn){
    ...
    int res = a.back(), size = a.size();
    for(int  i = 0; i < size-1; i++)
        if(a[i] >= a[i+1])
            res += a[i];
        else res -= a[i];
    return res;
}
```
Thank you for reading.
