---
title: "Sum by Factors"
date: 2021-01-18T16:50:30+07:00
tags: ["codewars", "eratosthene"]
---

## The problem:
Source: [codewars](https://www.codewars.com/kata/54d496788776e49e6b00052f)

Given an array of positive or negative integers $I = [i_1, .., i_n]$ , you have to produce a sorted array P of the form [ [$p$, sum of all $i_j$ of $I$ for which $p$ is a prime factor ($p$ positive) of $i_j$] ...]

P will be sorted by increasing order of the prime numbers. The final result has to be given as a string in Java, C#, C, C++ and as an array of arrays in other languages.

Example:
```
I = {12, 15}; // result = "(2 12)(3 27)(5 15)"
```
To solve this problem, we need to find prime numbers, from smallest to largest, for each found prime number, we've got:
- If $i_j$ divisible by $p$
    - $sum$ += $i_j$
## Solution
### Enumerate prime numbers
The fastest way to list them out is using [SoE](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes). We use this to find all prime numbers that are smaller than the largest absolute value of members in $I$ (i'll call it `max`). So, after input the array, we first find `max` and then sieve. After sieving, we'll save them into a vector to use them in process.
```cpp
std::vector<int> sieve(int n){
    std::vector<bool> isprime(n+1, 1);
    isprime[0] = isprime[1] = 0;
    for(int i = 2; i*i <= n; i++)
        if(isprime[i]==1)
            for(int j = i*i; j <= n; j += i)
                isprime[j]=0;
    std::vector<int> res;
    for(int i = 0; i < n+1; i++)
        if(isprime[i])
            res.push_back(i);
    return res;
};
```
### Processing
```cpp
for(auto i : primes){
    ans += '(';
    int sum = 0;
    for(auto j : lst)
        if(j % i == 0)
            sum += j;
    if(sum)
        ans = ans + to_string(i) + ' ' + to_string(sum);
    else{
        ans.pop_back();
        continue;
    }
    ans += ')';
}
Well done, with all the code above, we can submit and pass this kata UwU.
```