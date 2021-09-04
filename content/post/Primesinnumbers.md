---
title: "Phân tích thừa số nguyên tố"
date: 2021-01-23T20:46:06+07:00
tags: ["Số học"]
#ShowToc: true
TocOpen: true
---
## Bài toán 
Nguồn: [Codewars](https://www.codewars.com/kata/54d512e62a5e54c96200019e)
### Đọc hiểu
Cho một số dương $n$ > 1. Phân tích $n$ ra tích các số nguyên tố theo dạng:

($p_1$\*\*$n_1$)($p_2$\*\*$n_2$)...($p_k$\*\*$n_k$) 
- Trong đó: 
    - a**b nghĩa là $a^b$
    - $p_i$ liệt kê theo thứ tự tăng dần
    - Nếu $n_i$ = 1 thì không ghi ra

Ví dụ:
```
Input:  n = 86240
Output: (2**5)(5)(7**2)
```
## Lời giải 
Để phân tích ra thừa số nguyên tố, mỉnh đem chia số đó cho ước nguyên tố nhỏ nhất của nó. Cứ tiếp tục như vậy cho đến khi thương = 1.

Ví dụ: n = 160
|N  | I |
|---|---|
|160|2  |
|80 |2  |
|40 |2  |   
|20 |2  |
|10 |2  |
|5  |5  |
|1  |

Với ý tưởng trên, mỉnh có đoạn code sau:
```cpp
std::vector<int> fact;
int power = 0;
for(int i = 2; i*i < lst; i++){
    while(lst % i == 0){
		power++;
		lst /= i;
	}
}
```
Mỉnh không cần phải tìm trước các số nguyên tố mà chỉ cần 1 dòng `for` từ 2 đến $\sqrt{n}$ để xét tính chia hết là đủ. Bởi vì thương sẽ chia mãi cho đến khi không thể chia hết, đồng nghĩa với việc đã chia cả các hợp số nên không cần băn khoăn nữa UwU. Kết thúc vòng lặp thì `lst` là thừa số cuối cùng, nó có thể là 1 hoặc một số nguyên tố. Vì vậy, nếu nó khác 1 ta cần thêm vào chuỗi kết quả.
```cpp
std::string res = "";
std::vector<int> fact;
int power = 0;
for(int i = 2; i*i < lst; i++){
    while(lst % i == 0){
        power++;
        lst /= i;
    }
    if(power == 0)
        continue;
    res += "(" + std::to_string(i) + (power == 1 ? "" : ("**" + std::to_string(power))) + ")";
    power = 0;
    }   
    if(lst != 1)
        res += "(" + std::to_string(lst) + ")";
return res;
```
Cảm ơn bạn vì đã đọc.
## Bài viết liên quan:
[Phân tích thừa số nguyên tố của n!]({{< ref "/post/factorialdecomp" >}})
