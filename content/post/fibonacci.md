---
title: "Fibonacci"
date: 2021-02-12T04:39:42+07:00
tags: ["Ma trận", "Quy hoạch động"]
#ShowToc: true
TocOpen: true
---
## Bài toán
Nguồn: [Codeforces](https://codeforces.com/gym/102644/problem/C)

### Đọc hiểu
Tìm phần dư của số fibonacci thứ $n$ ($n <= 10^{18}$) cho $10^9 + 7$. Như vậy, mình cần tìm $F_n$ trogn dãy số được định nghĩa:
$$  
    \begin{aligned}
        F_0 & = 0\\\\
        F_1 & = 1\\\\
        F_i & = F_{i-1} + F_{i-2} (i >= 2)
    \end{aligned}
$$
Ví dụ:
```cpp
Input:  50
Output: 586268941
```
## Lời giải
### Cách 1: Quy hoạch động
Xem phương trình sai phân của dãy fibonacci là công thức truy hồi, mình có thể viết thành đoạn code sau:
```cpp
int f0 = 0, f1 = 1, fn;
for(int i = 2; i <= n; i++){
    fn = f0 + f1;
    f0 = f1;
    f1 = fn;
}
return fn;
```
Với độ phức tạp là $O(n)$, đoạn code này chạy ổn với $n <= 5. 10^6$. Tuy nhiên, $n$ có thể lên đến $10^{18}$ ở bài toán này nên mình cần một giải thuật tốt hơn, ít nhất phải là $O(log^2 n)$
### Cách 2: Nhân ma trận
Từ phương trình sai phân của dãy fibonacci, mình có thể viết lại như sau:
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
Có thể thấy F_n sẽ bằng dòng thứ 2 của $\begin{bmatrix} 1 & 1 \\\\ 1 & 0\end{bmatrix}^n$ nhân với 
$\begin{bmatrix} F_1 \\\\ F_0 \end{bmatrix}$. Mình lại có $F_1 = 1, F_0 = 0$ nên $F_n$ lúc này sẽ là phần tử ở dòng 2 cột 1 của $\begin{bmatrix} 1 & 1 \\\\ 1 & 0\end{bmatrix}^n$.
#### Tính lũy thừa với n lớn
Đối với $n$ lớn, thay vì cứ nhân cơ số $n$ lần theo định nghĩa, mình sẽ bình phương cơ số, đồng thời giảm số mũ đi một nửa. Ví dụ: Thay vì tính $2^6 = 2\times2\times2\times2\times2\times2,$ mình sẽ tính $2^6 = 4^4 = 8^2$

Nói cách khác, mình sẽ cố gắng thay thế việc nhân *nhiều lần* một số nhỏ thành nhân *vài lần* các số lớn với nhau.
```cpp
int res = 1;
while (b > 0){
    if(b % 2)
        res = res * a;
    a = a * a;
    b >>= 1
}
```
Hiện tại, cơ số của mình là một ma trận $2\times2$ nên mình cần khai báo `struct` và thêm hàm để thuận lợi cho việc tính toán cũng như lưu trữ:
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
    //cho res là ma trận đơn vị
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
Cuối cùng, ta cho $F = \begin{bmatrix} 1 & 1 \\\\ 1 & 0 \end{bmatrix}$, kết quả cuối cùng sẽ là `power(F,n).c`. 

Cảm ơn bạn vì đã đọc. Chúc bạn có một năm mới tràn đầy niềm vui và hạnh phúc =w=.