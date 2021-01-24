---
title: "T-Primes"
date: 2021-01-22T15:08:53+07:00
tags: ["Eratosthene"]
#ShowToc: true
TocOpen: true
---

## Bài toán
Nguồn: [Codeforces](https://codeforces.com/contest/230/problem/B)
### Đọc hiểu
Một số được gọi là t-prime nếu nó có đúng 3 ước số dương. Một số nguyên dương sẽ có 2 ước số là 1 và chính nó. Ngoài ra, nếu số đó, tạm gọi là $x$, có thêm 1 ước số nhỏ hơn $\sqrt{x}$ thì chắc chắn nó sẽ có ước khác. Để $x$ có đúng 3 ước số thì ước số nhỏ hơn $\sqrt{x}$ của nó phải là 1 số nguyên tố.

Nói cách khác, $x$ là t-prime nếu nó là **bình phương của 1 số nguyên tố.**

Trong bài toán này, ta sẽ kiểm tra 1 dãy $n$ các số $x$ rằng $x_i$ có phải là t-prime hay không.

Ví dụ
```
Input: 3
       4 5 6
Output: YES
        NO
        NO
```
## Lời giải
Đầu tiên, ta cần tìm tất cả các số nguyên tố từ 2 đến $\sqrt{x}$ bằng sàng [Eratosthene](https://vi.wikipedia.org/wiki/S%C3%A0ng_Eratosthenes). X giới hạn đến $10^{12}$ nên $\sqrt{x}$ lớn nhất sẽ là $10^6$.
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
Sau cùng, chỉ cần kiểm tra $\sqrt{x}$ có phải là số nguyên (không phải số thập phân :v) và là số nguyên tố hay không. Đơn giản phải không nào? OwO)/
```cpp
while (n--){
    long long x;
    cin >> x;
    long long q = sqrt(x);
    cout << (isprime[q] && q*q == x ? "YES" : "NO") << "\n"; 
}
```
## Cảm ơn bạn vì đã đọc.