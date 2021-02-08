---
title: "Phân tích thừa số nguyên tố của n!"
date: 2021-01-26T00:08:16+07:00
tags: ["Số học", "Eratosthene"]
#ShowToc: true
TocOpen: true
---
## Bài toán 
Nguồn: [Codewars](https://www.codewars.com/kata/5a045fee46d843effa000070)

Cho một số $n$ được nhập vào, việc của tụi mình là phân tích **giai thừa** của nó ra thừa số nguyên tố. 
Ví dụ:
```
Input:  n = 12
Output: 2^10 * 3^5 * 5^2 * 7 * 11
```
Có 1 lưu ý nho nhỏ là giá trị của $n$ có thể lên tới 4000, tất nhiên, 4000! sẽ trở thành 1 con số không hề nhỏ, nó có hơn 12000 chữ số lận á! ∑(O_O;)
## Lời giải
### Ý tưởng
Theo định nghĩa, giai thừa của $n$, kí hiệu là $n!$, là tích của $n$ số dương đầu tiên với nhau:\
$$12! = 1\times2\times3\times4\times5\times6\times7\times8\times9\times10\times11\times12$$

Như vậy, phân tích $n!$ cũng đồng nghĩa với việc phân tích từng thừa số của $n!$ rồi nhân các kết quả lại với nhau. Ví dụ: với $n = 12$ mình có bảng phân tích ứng với từng thừa số sau: 
| 12!  | 2 | 3 | 4   | 5 | 6        | 7 | 8   | 9   |10        | 11 | 12         |
|:-:   |:-:|:-:|:-:  |:-:|:-:       |:-:|:-:  |:-:  |:-:       |:-: |:-:         |
|decomp|$2$|$3$|$2^2$|$5$|$2\times3$|$7$|$2^3$|$3^2$|$2\times5$|$11$|$2^2\times3$|

Sau khi nhân các kết quả lại, mình được kết quả cuối cùng là tích thừa số nguyên tố của $n!$ mà mình cần tìm
$$2\times3\times2^2\times5\times2\times3\times7\times2^3\times3^2\times2\times5\times11\times2^2\times3 = 2^{10}\times3^5\times5^2\times7\times11$$
### Chương trình
Bước đầu tiên, mình khởi tạo 2 mảng một chiêu để lưu tính nguyên tố và giá trị của các thừa số
```cpp
bool prime[n+1];
int  value[n+1];
prime[0] = prime[1] = false;
for(int i = 2; i <= n; i++){
    prime[i] = true;
    value[i] = i;
}
```
Thông thường, mình dùng sàng Eratosthene để đánh dấu các số nguyên tố nhưng với một chút thay đổi, mình có thể phân tích một lượt các hợp số là bội của số nguyên tố tìm được. Như vậy sẽ tiết kiệm thời gian hơn so với mình phân tích từng số một rồi mới tổng hợp kết quả.
```cpp
for(int i = 2; i <= n; i ++){
    if(f[i].prime){
        int power = 1;
        for(int j = i+i; j <= n; j += i){ //Lưu ý: j = i+i chứ không còn là i*i
            f[j].prime = false;
            while(f[j].value % i == 0){
                f[j].value /= i;
                power++;
            }
        }
    }
}
```
Sau cùng, mình thêm vào vài dòng hỗ trợ cho việc output là có thể submit
```cpp
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
```
Cảm ơn bạn vì đã đọc.