---
title: "Hamming Number"
date: 2022-05-23T17:37:36+07:00
tags: ["Số học", "Quy hoạch động"]
#ShowToc: true
TocOpen: true
---
# Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/526d84b98f428f14a60008da/)
## Đọc hiểu
Số Hamming là một số dương có dạng $2^ỉ3^j5^k$ với $i, j, k$ là các số không âm.

Viết chương trình tính số Hamming nhỏ nhất thứ n.

Đặc biệt:
- Số Hamming đầu tiên là $1 = 2^03^05^0$
- Số Hamming thứ hai là $2 = 2^13^05^0$
- Số Hamming thứ ba là $3 = 2^03^15^0$
- Số Hamming thứ tư là $4 = 2^23^05^0$
- Số Hamming thứ năm là $5 = 2^03^05^1$

20 số Hamming nhỏ nhất đầu tiên được đưa vào trong test mẫu.

Chương trình cần phải tính toán được 5000 (Clojure: 2000, NASM: 13282) số Hamming nhỏ nhất mà không bị timeout.
# Lời giải
## Vét cạn
Phương pháp đơn giản nhất chính là lần lượt kiểm tra các số từ $2$ trở đi xem nó có phải số Hamming hay không và tăng biến đếm cho đến khi tìm được số Hamming thứ n. Để kiểm tra một số liệu có phải là số Hamming hay không thì dựa vào các điểm sau: 
- Số Hamming là một tích các lũy thừa cơ số 2, 3 và 5. 
- 2, 3 vả 5 là số nguyên tố.

Nói cách khác, số Hamming là một số có ước nguyên tố không lớn hơn 5. Mình sẽ thông qua việc [phân tích thừa số nguyên tố]({{< ref "post/Primesinnumbers">}}) để kiểm tra và việc phân tích sẽ chỉ dừng ở số 5, nếu phần còn lại của số cần kiểm tra bằng 1 - tức là không còn thừa số nào khác - thì số đó là số Hamming.

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
Tuy nhiên, phương pháp này sẽ dẫn đến mất thời gian để chạy chương trình, nhất là khi có một yêu cầu cụ thể rằng:
> Chương trình cần phải tính toán được 5000 (Clojure: 2000, NASM: 13282) số Hamming nhỏ nhất mà không bị timeout.
## Quy hoạch động
Do đặc điểm là một tích các lũy thừa cơ số 2, 3, và 5, số Hamming có thể được phân tách thành tích của một số Hamming nhỏ hơn với 2, 3, hoặc là 5: 
$$
2^ỉ3^j5^k = \Bigg[
\begin{array}{1}
    2^{i-1}3^j5^k \times 2 \\\\
    2^i3^{j-1}5^k \times 3 \\\\
    2^i3^j5^{k-1} \times 5 \\\\
\end{array}
$$
Như vậy, dãy số Hamming có thể được xây dựng như sau: 
$$
\begin{cases}
    H_1 = 1 \\\\
    H_x = min(H_i \times 2, H_j \times 3, H_k \times 5)
\end{cases}
$$
với $i, j, k$ là chỉ số của dãy Hamming ($i, j, k < x$). Các chỉ số này được tính từ $1$ và chỉ tăng khi bội của số Hamming ở vị trí tương ứng đã được thêm vào dãy. Ví dụ:
- $H_2 = min(H_1 \times 2, H_1 \times 3, H_1 \times 5) = min(1 \times 2, 1 \times 3, 1 \times 5) = 2$

    $\Rightarrow$ $i$ tăng lên 1 do tích $H_i \times 2$ được chọn: $i = 2$.

- $H_3 = min(H_2 \times 2, H_1 \times 3, H_1 \times 5) = min(2 \times 2, 1 \times 3, 1 \times 5) = 3$

    $\Rightarrow$ $j$ tăng lên 1.
- ...
- $H_6 = min(H_3 \times 2, H_2 \times 3, H_2 \times 5) = min(3 \times 2, 2 \times 3, 2 \times 5) = 6$

    $\Rightarrow$ $i$ và $j$ tăng lên 1.

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
Cảm ơn bạn vì đã đọc.
