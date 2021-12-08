---
title: "Số phân số tối giản với mẫu số d"
date: 2021-12-08T17:49:22+07:00
tags: ["Số học"]
#ShowToc: true
TocOpen: true
---
## Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/55b7bb74a0256d4467000070/)
### Đọc hiểu
Với $n$ là tử số và $d$ là mẫu số, phân số được định nghĩa là tối giản nếu và chỉ nếu $GCD(n,d) = 1$.

Ví dụ, $\displaystyle\frac{5}{16}$ là phân số tối giản, trong khi $\displaystyle\frac{6}{16}$ không phải vì cả 6 và 16 đều chia hết cho 2 nên phân số rút gọn thành $\displaystyle\frac{3}{8}$

Cho một số $d$, hỏi có bao nhiêu phân số thực sự (phân số có tử bé hơn mẫu) tối giản nếu dùng $d$ làm mẫu số?

Ví dụ, cho $d = 15$: có tổng cộng 8 phân số thực sự tối giản mang giá trị từ 0 đến 1 là $$\displaystyle\frac{1}{15},\frac{2}{15},\frac{4}{15},\frac{7}{15},\frac{8}{15},\frac{11}{15},\frac{13}{15},\frac{14}{15}$$

Yêu cầu: xây dựng một hàm tính xem có bao nhiêu phân số thực sự tối giản với mẫu số đã cho:
```
properFractions(1)  ==  0
properFractions(2)  ==  1
properFractions(5)  ==  4
properFractions(15) ==  8
properFractions(25) == 20
```
Và hàm phải xử lí được số lớn.

(`proper` là do người ra đề viết nhầm với `reduced`)

## Lời giải
### Chạy trâu
Bài toán có thể vét cạn: với mỗi số $n \leq d$ tính GCD của chúng đồng thời tăng biến đếm nếu GCD = 1. GCD (hay ước chung lớn nhất) của 2 số được tính bằng giải thuật Euclid.
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
Tuy nhiên, đề bài cho biết $d$ có thể là một số lớn nên phương pháp này không khả thi.
### Hàm phi Euler
Vì bài toán chỉ xét các phân số thực sự ($n < d$) nên bài toán trở thành `Tính hàm phi Euler của d`.

> Trong lý thuyết số, hàm số Euler của một số nguyên dương $n$ được định nghĩa là số các số nguyên dương nhỏ hơn hoặc bằng $n$ [nguyên tố cùng nhau](https://vi.wikipedia.org/wiki/Số_nguyên_tố_cùng_nhau) với $n$. Hàm Euler được ký hiệu bởi $\phi (n)$ hoặc $\varphi (n)$, do đó hàm được gọi làm hàm phi Euler.\
*[Wikipedia](https://vi.wikipedia.org/wiki/Hàm_phi_Euler)*

Hàm phi Euler được tính bằng công thức:
$$\begin{aligned}
\phi(n) &= n \times (1 - \frac{1}{p_1}) \times (1 - \frac{1}{p_2}) \times...\times (1 - \frac{1}{p_k})\\\\
&= n\prod_{i=1}^k(1 - \frac{1}{p_i})
\end{aligned}$$

trong đó, $\ p_i$ là các thừa số nguyên tố khác nhau của $n$.

Kết hợp giải thuật [phân tích thừa số nguyên tố của n]({{< ref "/post/Primesinnumbers" >}}) với công thức trên, mình có được lời giải cho bài toán:
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
Cảm ơn bạn vì đã đọc
