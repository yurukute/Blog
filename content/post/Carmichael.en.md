---
title: "Carmichael function"
date: 2021-09-04T05:23:34+07:00
tags: ["Numbers"]
#ShowToc: true
TocOpen: true
---
## Problem 
Source: [Codewars](https://www.codewars.com/kata/53c92d34689f84a2d5000e48)

In number theory, the Carmichael function of a positive integer $n$, denoted $\lambda(n)$, is defined as the smallest positive integer m such that
$$a^m \equiv 1 \mod n$$
for every integer $a \leq n$ that is coprime to $n$. The Carmichael function is also known as the reduced totient function (as it is linked to Euler Totient function) or the least universal exponent function. The Carmichael function is important in number theory.

For example let $n = 8$. The list of integer $a \leq 8$, coprime to $8: 1, 3, 5, 7.$
$$
\begin{aligned}
	1^1 \equiv 1 \mod 8 \\\\
	3^1 \equiv 3 \mod 8 \\\\
\end{aligned}
$$
So $\lambda(8) > 1.$
$$
\begin{aligned}
	1^2 \equiv 1 \mod 8 \\\\
	3^2 \equiv 1 \mod 8 \\\\
	5^2 \equiv 1 \mod 8 \\\\
	7^2 \equiv 1 \mod 8 \\\\
\end{aligned}
$$

So $\lambda(8) = 2.$

There is generally two approach to this function:

 - Iteratively checking $a^m \equiv 1 \mod n$ for growing $m$ and every $a \leq n$, coprime to $n$. This may not work for big numbers.

 - Using an exact formula like for Euler Totient. This is an explicit formula for calculating $\lambda(n)$ depending on the prime decomposition of $n$.

For the second method the formula is deduced from the lcm formula for $\lambda$ and the totient function (see the associated kata):
 - lcm formula, where $p_1,...,p_n$ are the prime factor of $n$, $w_1,...,w_n$, the powers associated.:
$$
\begin{aligned}
 	\lambda(n) & = \lambda(p_1^{w_1} \times p_2^{w_2} \times ... \times p_n^{w_n}) \\\\
 	\lambda(n) & = lcm(\lambda(p_1^{w_1}), \lambda(p_2^{w_2}),..., \lambda(p_n^{w_n})) \\\\
\end{aligned}
$$
Then for p prime, we have a link between $\lambda(p^w)$ and $\phi(p^w)$, the Euler Totient function:
$$
\begin{aligned}
 	\lambda(p^w) &= \phi(p^w) 				& p &> 2\\\\
 	\lambda(p^w) &= \phi(p^w) 				& p &= 2, w < 3\\\\
 	\lambda(p^w) &= \frac{1}{2} \phi(p^w)	& p &= 2, w \geq 3
\end{aligned}
$$

You have to code `Carmichael` function, that take an integer $1 ≤ n$ as input and return $\lambda(n)$. You also have to check if $n$ is a number, integer and that $1 ≤ n$, if it is not the case, the function should return `0`.

Input range: $1 ≤ n ≤ 1e10$
## Solution
Euler Totient function of $n = p_1^{w_1} \times p_2^{w_2} \times...\times p_k^{w_n}$ is calculated by formula:
$$ \phi(n) = n \times (1 - \frac{1}{p_1}) \times (1 - \frac{1}{p_2}) \times...\times (1 - \frac{1}{p_k}) $$

Given $n = p^w$, the fomula can be rewritten:
$$ \phi(n) = p^w \times (1 - \frac{1}{p}) = p^{w-1} \times (p-1)$$

Associate [prime factorization]({{< ref "/post/Primesinnumbers" >}}) of n with finding their $\lambda$ via $\phi$ i have the code:
```cpp
#include <vector>
#include <math.h>

long long Carmichael(long long n){
	if (n < 1) return 0;
	if (n == 1) return 1;
	std::vector<long long> factors;
	for(long long i = 2; i*i <= n; i += 2){
		long long w = 0;
		while(n % i == 0){
			w++;
			n /= i;
		}
		if (i == 2 && w >= 3)
			factors.push_back((pow(i, w-1) * (i-1))/2);
		else if(i >= 2 && w > 0)
			factors.push_back(pow(i, w-1) * (i-1));
		if(i == 2) i--;
	}
	if(n != 1) factors.push_back(n-1);
}
```
I now have the result of  $\lambda(p_1^{w_1}), \lambda(p_2^{w_2}),..., \lambda(p_n^{w_n})$. Last step is caculating their least common multipler. The simpliest method is divide their product by their greatest common divisor:
$$
LCM(a, b) = \frac{a \times b}{GCD(a,b)} = a \times \frac{b}{GCD(a, b)}
$$

By `a` represent needed result and `b`, the element $\lambda(p_i^{w_i})$ in `factors`, I can operate a loop like this:

```cpp
long long res = 1;
for(auto i : factors)
	res *= i/gcd(res, i);
return res;
```
`gcd` function with [Euclid algorithm](https://en.wikipedia.org/wiki/Euclidean_algorithm#Procedure)
```cpp
long long gcd(int a, int b) {
	while(b){
		long long t = a % b;
		a = b;
		b = t;
	}
	return a;
}
```
Thank you for reading.
