
---
title: "Hamming Number"
date: 2022-05-23T17:37:36+07:00
tags: ["Math", "Dynamic programing"]
#ShowToc: true
TocOpen: true
---
# Problem
Source: [Codewars](https://www.codewars.com/kata/526d84b98f428f14a60008da/)

A [Hamming number](https://en.wikipedia.org/wiki/Regular_number) is a positive integer of the form 2i3j5k, for some non-negative integers i, j, and k.

Write a function that computes the nth smallest Hamming number.

Specifically:

The first smallest Hamming number is 1 = 203050
The second smallest Hamming number is 2 = 213050
The third smallest Hamming number is 3 = 203150
The fourth smallest Hamming number is 4 = 223050
The fifth smallest Hamming number is 5 = 203051
The 20 smallest Hamming numbers are given in example test fixture.

Your code should be able to compute all of the smallest 5,000 (Clojure: 2000, NASM: 13282) Hamming numbers without timing out.

# Solution
## Brute force
The simplest method is to check wether a number from $2$ on is Hamming number or not, then increase counter until the nth Hamming number is found. To check if a number is a Hamming number relies on its traits:
- Hamming number is the product of the exponents of 2, 3 and 5. 
- 2, 3 and 5 are prime numbers.

In other words, checking the number can be achieved by [factoring it]({{< ref "post/Primesinnumbers">}}) and the factoring process stopping only at 5, if the leftover is equal to 1, which means there's no other factors, that's a Hamming number.

```cpp
#include <cstdint>

int isHamber(uint64_t n){
    for(int i = 2; i <= 5; i++)
        while (n % i == 0)
            n /= i;
    return n == 1;
}

uint64_t hamber(int n){
    uint64_t res = 0;
    int count = 0;
    while(count < n){
        res++;
        if(isHamber(res))
            count++;
    }
    return res;
}
```
However, this method is time consuming, especially when there's specific requirement
> Your code should be able to compute all of the smallest 5,000 (Clojure: 2000, NASM: 13282) Hamming numbers without timing out.
## Dynamic programing
Because of being the product of the exponents of 2, 3 and 5, Hamming number can be decomposed into a smaller Hamming number and 2, 3 or 5.
$$
2^ỉ3^j5^k = \Bigg[
\begin{array}{1}
    2^{i-1}3^j5^k \times 2 \\\\
    2^i3^{j-1}5^k \times 3 \\\\
    2^i3^j5^{k-1} \times 5 \\\\
\end{array}
$$
Thus, Hamming seqsequence can be formed by this: 
$$
\begin{cases}
    H_1 = 1 \\\\
    H_x = min(H_i \times 2, H_j \times 3, H_k \times 5)
\end{cases}
$$
where $i, j, k$ are Hamming seqsequence's indexes ($i, j, k < x$). They are counted from $1$ and increased only when the multiple of the Hamming number in the corresponding positions is added to the seqsequence. For example:
- $H_2 = min(H_1 \times 2, H_1 \times 3, H_1 \times 5) = min(1 \times 2, 1 \times 3, 1 \times 5) = 2$

    $\Rightarrow$ $i$ is increased by one for $H_i \times 2$ is chosen: $i = 2$.

- $H_3 = min(H_2 \times 2, H_1 \times 3, H_1 \times 5) = min(2 \times 2, 1 \times 3, 1 \times 5) = 3$

    $\Rightarrow$ $j$ is increased by one.
- ...
- $H_6 = min(H_3 \times 2, H_2 \times 3, H_2 \times 5) = min(3 \times 2, 2 \times 3, 2 \times 5) = 6$

    $\Rightarrow$ $i$ and $j$ are increased by one.

```cpp
#include <cstdint>
#include <algorithm>

uint64_t hamber(int n){
    uint64_t hambers[13282], next2 = 2, next3 = 3, next5 = 5;
    int i = 0, j = 0, k = 0;
    hambers[0] = 1;
    for(int x = 1; x < n; x++){
        hambers[x] = std::min(std::min(next2, next3), next5);
        if(hambers[x] == next2)
            next2 = hambers[++i]*2;
        if(hambers[x] == next3)
            next3 = hambers[++j]*3;
        if(hambers[x] == next5)
            next5 = hambers[++k]*5;
    }
    return hambers[n-1];
}
```
Thank you for reading.
