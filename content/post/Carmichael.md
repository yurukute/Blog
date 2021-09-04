---
title: "Hàm Carmichael"
date: 2021-09-04T05:23:34+07:00
tags: ["Số học"]
#ShowToc: true
TocOpen: true
---
## Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/53c92d34689f84a2d5000e48)
### Đọc hiểu
Trong lý thuyết số, hàm Carmichael của một số nguyên dương $n$, ký hiệu $\lambda(n)$, là số nguyên dương m nhỏ nhất sao cho $a^m \equiv 1 \mod n$ với mọi $a \leq n$ là các số nguyên tố cùng nhau với n. 

Ví dụ $n = 8$. Các số nguyên tố cùng nhau với $8$ không vượt quá $8: 1, 3, 5, 7.$
$$
\begin{aligned}
	1^2 \equiv 1 \mod 8 \\\\
	3^2 \equiv 1 \mod 8 \\\\
	5^2 \equiv 1 \mod 8 \\\\
	7^2 \equiv 1 \mod 8 \\\\
\end{aligned}
$$

Vì vậy, $\lambda(8) = 2$.

Có 2 cách để tính hàm này:
 - Với mỗi số nguyên tố cùng nhau với n không vượt quá n, kiểm tra $a^m \equiv 1 \mod n$ với $m$ tăng dần. Cách này không hiệu quả đối với số lớn.
 - Dùng công thức chính xác như hàm phi Euler. Đây là công thức tường minh để tính $\lambda(n)$ dựa trên phân tích thừa số nguyên tố của n.
 
Ở phương pháp thứ hai, công thức được suy ra từ công thức bcnn của $\lambda$ và hàm phi: 
 - Cho $p_1,...,p_n$ là các thừa số nguyên tố của n với các số mũ $w_1,...,w_n$ của chúng:
$$
\begin{aligned}
 	\lambda(n) & = \lambda(p_1^{w_1} \times p_2^{w_2} \times ... \times p_n^{w_n}) \\\\
 	\lambda(n) & = lcm(\lambda(p_1^{w_1}), \lambda(p_2^{w_2}),..., \lambda(p_n^{w_n})) \\\\
\end{aligned}
$$
Đối với mỗi số nguyên tố p, ta có sự liên kết giữa $\lambda(p^w)$ và hàm phi Euler $\phi(p^w)$:
$$
\begin{aligned}
 	\lambda(p^w) &= \phi(p^w) 				& p &> 2\\\\
 	\lambda(p^w) &= \phi(p^w) 				& p &= 2, w < 3\\\\
 	\lambda(p^w) &= \frac{1}{2} \phi(p^w)	& p &= 2, w \geq 3
\end{aligned}
$$

Viết hàm `Carmichael`, nhận vào một số nguyên $n \geq 1$ và trả về $\lambda(n)$. Nếu $n < 1$ trả về `0`

Khoảng nhập: $1 \leq n \leq 1e10$
## Lời giải
Hàm phi Euler của $n = p_1^{w_1} \times p_2^{w_2} \times...\times p_k^{w_n}$ được tính bởi công thức sau:
$$ \phi(n) = n \times (1 - \frac{1}{p_1}) \times (1 - \frac{1}{p_2}) \times...\times 1 - \frac{1}{p_k} $$

Cho $n = p^w$ công thức được viết lại như sau:
$$ \phi(n) = p^w \times (1 - \frac{1}{p}) = p^{w-1} \times (p-1)$$

Kết hợp vừa [phân tích thừa số nguyên tố]({{< ref "/post/Primesinnumbers" >}}) của n, vừa tính $\lambda$ của chúng thông qua $\phi$ mình có đoạn chương trình sau:
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
Lúc này, mình đã có được một dãy các kết quả của $\lambda(p_1^{w_1}), \lambda(p_2^{w_2}),..., \lambda(p_n^{w_n})$. Công đoạn cuối là tính bội chung nhỏ nhất của chúng. Một trong những phương pháp đơn giản là lấy tích chia cho ước chung lớn nhất:
$$
LCM(a, b) = \frac{a \times b}{GCD(a,b)} = a \times \frac{b}{GCD(a, b)}
$$

Với `a` là kết quả cần tính và `b` là các phần tử $\lambda(p_i^{w_i})$ trong `factors`, mình có thể khởi động vòng lặp tính `lcm` như sau:

```cpp
long long res = 1;
for(auto i : factors)
	res *= i/gcd(res, i);
return res;
```
Hàm `gcd` sử dụng [giải thuật Euclid](https://vi.wikipedia.org/wiki/Giải_thuật_Euclid#Thuật_toán)
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
Cảm ơn bạn vì đã đọc.
