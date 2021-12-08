---
title: "Number of reduced fractions with denominator d"
date: 2021-12-08T17:49:22+07:00
tags: ["Maths", "Fundamentals"]
#ShowToc: true
TocOpen: true
---
## Problem
If n is the numerator and d the denominator of a fraction, that fraction is defined a reduced fraction if and only if GCD(n,d)==1.

For example $\displaystyle\frac{5}{16}$ is a reduced fraction, while $\displaystyle\frac{5}{16}$ is not, as both 6 and 16 are divisible by 2, thus the fraction can be reduced to $\displaystyle\frac{3}{8}$.

Now, if you consider a given number d, how many reduced fractions can be built using d as a denominator?

For example, let's assume that d is 15: you can build a total of 8 different proper fractions between 0 and 1 with it: $$\displaystyle\frac{1}{15},\frac{2}{15},\frac{4}{15},\frac{7}{15},\frac{8}{15},\frac{11}{15},\frac{13}{15},\frac{14}{15}$$

You are to build a function that computes how many proper fractions you can build with a given denominator:
```
properFractions(1)  ==  0
properFractions(2)  ==  1
properFractions(5)  ==  4
properFractions(15) ==  8
properFractions(25) == 20
```
Be ready to handling big numbers.

(Author used `proper` instead of `reduced`, which is improper, by mistake)
## Solution
### Brute force
This problem can be solved with brute force method: For each $n \leq d$, compute their GCD and increase counter by one if GCD = 1. GCD (Greatest common divisor) of two intergers is computed by Euclid's algorithm.
```cpp
long gcd(long a, long b) {
	while(b){
		int t = a % b;
		a = b;
		b = t;
	}
	return a;
}

long properFractions(long d)
{
	int count = 0;
	for(int n = 1; n < d; n++)
		if(gcd(n,d) == 1)
		count++;
	return count;
}
```
However, $d$ is mentioned to mightly be a big number so this approach can't be used.
### Euler's totient function
Because this problem only consider proper fractions ($n < d$), the problem become `Computing Euler's totient function of  d`.

>In number theory, Euler's totient function counts the positive integers up to a given integer $n$ that are [relatively prime](https://en.wikipedia.org/wiki/Coprime_integers) to $n$. It is written using the Greek letter phi as $\phi (n)$ or $\varphi (n)$, and may also be called Euler's phi function.\
*[Wikipedia](https://en.wikipedia.org/wiki/Euler's_totient_function)*

Euler's product formula:
$$\begin{aligned}
\phi(n) &= n \times (1 - \frac{1}{p_1}) \times (1 - \frac{1}{p_2}) \times...\times (1 - \frac{1}{p_k})\\\\
&= n\prod_{i=1}^k(1 - \frac{1}{p_i})
\end{aligned}$$

where $p_i$ is the distinct prime numbers dividing $n$.

Had combined [prime factor decomposition of n]({{< ref "/post/Primesinnumbers" >}}) algorithm with the formula above, I got solution for this:
```cpp
long properFractions(long n)
{
	if(n == 1)
		return 0;
	long res = n;
	for(long i = 2; i*i <= n; i++){
		if(n % i == 0){
			while (n % i == 0)
				n /= i;
			res -= res/i;
		}
	}
	if (n > 1)
		res -= res/n;
	return res;
}
```
Thank you for reading.
