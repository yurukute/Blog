---
title: "T-Primes"
date: 2021-01-22T15:39:07+07:00
tags: ["Eratosthene"]
#ShowToc: true
TocOpen: true
---

## Promblem
Source: [Codeforces](https://codeforces.com/contest/230/problem/B)

We know that prime numbers are positive integers that have exactly two distinct positive divisors. Similarly, we'll call a positive integer $t$ Т-prime, if $t$ has exactly three distinct positive divisors.

You are given an array of $n$ positive integers. For each of them determine whether it is Т-prime or not.

Examples
```
Input:  3
        4 5 6
Output: YES
        NO
        NO
```
## Solution
First, we need to find all prime numbers from 2 to $\sqrt{x}$. We'll use [SoE](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) to do that because we've known the $x$'s limit is $10^{12}$, therefore, the maximum value of $\sqrt{x}$ is $10^{6}$
```cpp
int limit = 1000001
std::vector<bool> isprime(limit, true);
isprime[0] = isprime[1] = false;
for(int i = 2; i < limit; i++)
    isprime[i] = true;
for(int i = 2; i*i < limit; i++){
    if(isprime[i])
        for(long long j = i * i; j < limit; j += i)
            isprime[j] = false;
}
```
The last step is checking whether $\sqrt{x}$ is an integer and a prime number or not. It's simple, isn't it? OwO)/
```cpp
while (n--){
    long long x;
    cin >> x;
    long long q = sqrt(x);
    cout << (isprime[q] && q*q == x ? "YES" : "NO") << "\n"; 
}
```
Thank you for reading.