---
title: "Sum by Factors"
date: 2021-01-18T16:50:30+07:00
tags: ["Eratosthene"]
---

## Bài toán:
Nguồn: [Codewars.com](https://www.codewars.com/kata/54d496788776e49e6b00052f)
### Đọc hiểu:
Input: Một mảng các số nguyên: $I = [i_1, i_2 ,..., i_n]$

Output: một chuỗi theo dạng ($p_1$ $sum_1$) ($p_2$ $sum_2$) ... ($p_x$ $sum_m$)

Trong đó:
- $p$ : số nguyên tố
- $sum$ : tổng các số trong mảng I chia hết cho p

Ví dụ:
```
I = {12, 15}; //kết quả: "(2 12)(3 27)(5 15)"
```
Như vậy, để giải được bài này ta cần tìm các số nguyên tố từ nhỏ đến lớn, ứng với mỗi số nguyên tố, ta có:
- Nếu số $i_j$ chia hết cho $p$
    - $sum$ += $i_j$
## Lời giải
### Liệt kê các số nguyên tố
Cách liệt kê nhanh nhất là dùng sàng [Eratosthene](https://vi.wikipedia.org/wiki/S%C3%A0ng_Eratosthenes). Chúng ta sẽ tìm tất cả các số nguyên tố nhỏ hơn giá trị tuyệt đối lớn nhất trong mảng (`max`). Vì vậy, việc đầu tiên cần làm sau khi nhập mảng là tìm `max`, lưu lại các số nguyên tố bé hơn `max` để dùng cho bước xử lí.
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
### Bắt đầu xử lí
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
```
Thế là xong! Ghép lại là có thể submit UwU. 

Cảm ơn bạn vì đã đọc