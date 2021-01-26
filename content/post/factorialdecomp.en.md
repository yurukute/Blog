---
title: "Factorial decomposition"
date: 2021-01-26T00:08:20+07:00
tags: ["Numbers", "Eratosthene"]
#ShowToc: true
TocOpen: true
---
## Problem
Source: [Codewars](https://www.codewars.com/kata/5a045fee46d843effa000070)

The aim of the kata is to decompose $n!$(factorial n) into its prime factors.
For example:
```
Input:  n = 12
Output: 2^10 * 3^5 * 5^2 * 7 * 11
```

Note that $n$ can reach 4000 and, of course, 4000! would be very big with more than 12000 digits ∑(O_O;)

## Solution
### Idea
By definition, the factorial of a positive integer $n$, denoted by $n!$, is the product of all positive integers less than or equal to $n$:
$12! = 1\times2\times3\times4\times5\times6\times7\times8\times9\times10\times11\times12$

Thus, decomp $n!$ also means decomp each factors of $n!$ then multiply them. For example, $n = 12$, we can decomp each factor as the table below:
| 12!  | 2 | 3 | 4   | 5 | 6        | 7 | 8   | 9   |10        | 11 | 12         |
|:-:   |:-:|:-:|:-:  |:-:|:-:       |:-:|:-:  |:-:  |:-:       |:-: |:-:         |
|decomp|$2$|$3$|$2^2$|$5$|$2\times3$|$7$|$2^3$|$3^2$|$2\times5$|$11$|$2^2\times3$|

After we multiply them, the final result is the prime factor decomposition of a $n!$ which we need to find
$2\times3\times2^2\times5\times2\times3\times7\times2^3\times3^2\times2\times5\times11\times2^2\times3 = 2^{10}\times3^5\times5^2\times7\times11$
### Program
First of all, we initialize 2 arrays to save the factors and their primality
```cpp
bool prime[n+1];
int  value[n+1];
prime[0] = prime[1] = false;
for(int i = 2; i <= n; i++){
    prime[i] = true;
    value[i] = i;
}
```
Generally, we use Eratosthene to find the prime numbers, now, with some changes, we can decomp all composite numbers which are multiples of found prime.
```cpp
for(int i = 2; i <= n; i ++){
    if(f[i].prime){
        int power = 1;
        for(int j = i+i; j <= n; j += i){ //In this line, j = i+i not i*i
            f[j].prime = false;
            while(f[j].value % i == 0){
                f[j].value /= i;
                power++;
            }
        }
    }
}
```
Finally, we add some code for output
```cpp
std::string decomp(int n) {
    bool prime[n+1];
    int  value[n+1];
    prime[0] = prime[1] = false;
    for(int i = 2; i <= n; i++){
        prime[i] = true;
        value[i] = i;
    }
    for(int i = 2; i <= n; i ++){
        if(f[i].prime){
            res += (" * " + std::to_string(i));
            int power = 1;
            for(int j = i+i; j <= n; j += i){
                prime[j] = false;
                while(value[j] % i == 0){
                    value[j] /= i;
                    power++;
                }
            }
            if(power > 1)
                res += ("^" + std::to_string(power));
        }
    }
    res.erase(res.begin(), res.begin()+3);
    return res;
}
```
And done! UwU)/
## Thank you for reading.
