---
title: "Primes in numbers"
date: 2021-01-23T20:46:09+07:00
tags: ["Numbers"]
#ShowToc: true
TocOpen: true
---
## Problem
[Codewars](https://www.codewars.com/kata/54d512e62a5e54c96200019e)

Given a positive number n > 1 find the prime factor decomposition of n. The result will be a string with the following form:

($p_1$\*\*$n_1$)($p_2$\*\*$n_2$)...($p_k$\*\*$n_k$)

where 
- a**b means $a^b$
- $p_i$ in increasing order
- $n_i$ empty if n(i) is 1.

Example: 
```
Input:  n = 86240 
Output: (2**5)(5)(7**2)(11)
```
## Solution
Generally, in order to calculate all of the prime factors of a number, you have to go about dividing the original number by its smallest prime factor. We'll repeat the steps until reaching 1.

For example: n = 160
|N  | I |
|---|---|
|160|2  |
|80 |2  |
|40 |2  |   
|20 |2  |
|10 |2  |
|5  |5  |
|1  |

The idea give us code like this: 
```cpp
std::vector<int> fact;
int power = 0;
for(int i = 2; i*i < lst; i++){
	while(lst % i == 0){
		power++;
		lst /= i;
	}
}
```
In coding, we don't need to find those prime factors before. Just a `for` loop from 2 to $\sqrt{n}$ would be enough. Because we repeat the division until the quotient is not divisible, that means, we divided the composite numbers UwU. After the loop finished, `lst`, the remain factor, would be either 1 or a prime number. Thus, add it to the result string if it is not 1.
```cpp
std::string res = "";
std::vector<int> fact;
int power = 0;
for(int i = 2; i*i < lst; i++){
    while(lst % i == 0){
        power++;
        lst /= i;
    }
    if(power == 0)
        continue;
    res += "(" + std::to_string(i) + (power == 1 ? "" : ("**" + std::to_string(power))) + ")";
    power = 0;
    }   
    if(lst != 1)
        res += "(" + std::to_string(lst) + ")";
return res;
```
Thank you for reading.
## Related post
[Factorial decomp]({{< ref "/post/factorialdecomp" >}})
