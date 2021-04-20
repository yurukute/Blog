---
title: "Fibonacci"
date: 2021-02-12T04:39:49+07:00
tags: ["Matrix", "Dynamic Programing"]
#ShowToc: true
TocOpen: true
---
## Problem
Source: [Codeforces](https://codeforces.com/gym/102644/problem/C)

Find the $n^th$ [Fibonacci](https://en.wikipedia.org/wiki/Fibonacci_number) number modulo $10^9+7$. So, you need to find $F_n$ in the sequence defined as
$$  
    \begin{aligned}
        F_0 & = 0\\\\
        F_1 & = 1\\\\
        F_i & = F_{i-1} + F_{i-2} (i >= 2)
    \end{aligned}
$$
Example:
```cpp
Input:  50
Output: 586268941
```
## Solution
### Approach 1: Dynamic Programing
Using fibonacci's difference equation as a recurrence relation, i can write to a program like this:
```cpp
int f0 = 0, f1 = 1, fn;
for(int i = 2; i <= n; i++){
    fn = f0 + f1;
    f0 = f1;
    f1 = fn;
}
return fn;
```
This code's complexity is $O(n)$ which is fine with $n <= 5. 10^6$. However, $n$ is up to $10^{18}$ in this problem so i need a better way, must be at least $O(log^2 n)$ complexity.
### Approach 2: Matrix exponentiation
I can rewrite its different equation as
$$
\begin{aligned}
    F_{n+1} & = 1\\ F_n + 1\\ F_{n-1}\\\\
    F_{n}   & = 1\\ F_n + 0\\ F_{n-1}\\\\
    \\\\
    => \begin{bmatrix} F_{n+1} \\\\ F_n \end{bmatrix} & = 
    \begin{bmatrix} 1 & 1 \\\\ 1 & 0 \end{bmatrix} \begin{bmatrix} F_n \\\\ F_{n-1} \end{bmatrix}
    \\\\\\\\ & =
    \begin{bmatrix} 1 & 1 \\\\ 1 & 0 \end{bmatrix}^n \begin{bmatrix} F_1 \\\\ F_0 \end{bmatrix}
\end{aligned}
$$
As you can see, F_n is equal to $2^{nd}$ row $\begin{bmatrix} 1 & 1 \\\\ 1 & 0\end{bmatrix}^n$ multiply by
$\begin{bmatrix} F_1 \\\\ F_0 \end{bmatrix}$. Also, $F_1 = 1, F_0 = 0$ so $F_n$ is now a value at $2^{nd}$ row and $1^{st}$ column of $\begin{bmatrix} 1 & 1 \\\\ 1 & 0\end{bmatrix}^n$.
### Large n power
I'll calculate power with large $n$ by square the base and halve the exponent rather than multiply the base $n$ times as definition. For example, instead of calculate $2^6 = 2\times2\times2\times2\times2\times2,$ i'll do $2^6 = 4^4 = 8^2$

In other words, i'll try to replace multiplying a small *many times* to multiplying 2 or 3 times large numbers. By this, i can reduce the complexity to $O(logn)$
```cpp
int res = 1;
while (b > 0){
    if(b % 2)
        res = res * a;
    a = a * a;
    b >>= 1
}
```
Now, the base is not a number but a $2 \times 2$ matrix so i need to declare a `struct`, so does the funtions for calculating and storing:
```cpp
const unsigned long long mod = 1e9 + 7;

struct matrix{
    unsigned long long a, b, c, d;
};

matrix multiply(matrix A, matrix B){
    matrix C;
    C.a = (A.a*B.a + A.b*B.c) % mod;
    C.b = (A.a*B.b + A.b*B.d) % mod;
    C.c = (A.c*B.a + A.d*B.c) % mod;
    C.d = (A.c*B.b + A.d*B.d) % mod;
    return C;
}

matrix power(matrix a, long long b){
    matrix res;
    //res is an identity matrix
    res.a = res.d = 1;
    res.b = res.c = 0;
    while (b > 0){
        if(b % 2)
            res = multiply(res, a);
        a = multiply(a, a);
        b >>= 1;
    }
    return res;
}
```
Finally, for $F = \begin{bmatrix} 1 & 1 \\\\ 1 & 0 \end{bmatrix}$, the final result is `power(F,n).c`. 

Thank you for reading.