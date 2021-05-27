---
title: "Hai con mã"
date: 2021-05-27T18:13:29+07:00
tags: ["Toán học" ]
#ShowToc: true
TocOpen: true
---
## Bài toán
Nguồn: [CSES](https://cses.fi/problemset/task/1072)

In ra tổng số cách đặt hai con mã lên bàn cờ $k \times k, k = 1,2,...,n.$ Với $n$ nhập từ bàn phím.
Ví dụ:
```
Input: 8
Output: 0
        6
        28
        96
        252
        550
        1056
        1848
``` 
## Lời giải
### Ý tưởng
Phương pháp đơn giản là ứng với mỗi vị trí của con mã thứ nhất, mình tìm các vị trí thích hợp cho con mã thứ hai rồi trả về tổng số vị trí tìm được. Thực hiện việc tìm kiếm $n$ lần cho các bàn cờ từ $1 \times 1$ đến $n \times n$ khá là liều lĩnh khi có nhiều vòng lặp lồng nhau nên mình cần cách khôn ngoan hơn 👀

Thay vì tìm trực tiếp như trên, mình có thể tiến hành tìm gián tiếp bằng cách lấy `tổng số cách đặt hai con mã vào bàn cờ` **trừ đi** `số cách đặt hai con mã để chúng ăn nhau`. 

`Tổng số cách đặt hai con mã vào bàn cờ $k \times k$` là:
$$C^2_{k^2} = \frac{k^2!}{2!(k^2-2)!} = \frac{k^2(k^2-1)(k^2-2)!}{2(k^2-2)!} = \frac{k^2(k^2-1)}{2}$$
Để ý rằng, khi hai con mã ở vị trí có thể ăn nhau, chúng tạo thành đường chéo của hình chữ nhật $2 \times 3$ (nằm ngang) hoặc $3 \times 2$ (nằm dọc). Bàn cờ là hình vuông nên số lượng hình chữ nhật $2 \times 3$ sẽ bằng với $3 \times 2$, vì thế ta chỉ việc đếm một loại rồi nhân đôi. 

Các bước đếm được tiến hành như sau:
- Bắt đầu từ ô trái trên cùng, mình vẽ ra một hình chữ nhật $2 \times 3$.
- Dịch hình chữ nhật sang phải 1 ô đến khi không thể dịch nữa. Vì 2 cột phải cùng của hình chữ nhật ko thể lọt ra ngoài bàn cờ nên mình dịch được $n-2$ lần.
- Tương tự, số lần mình dịch hình chữ nhật xuống dưới sẽ là $n-1$ (bởi hàng dưới cùng không thể lọt ra ngoài).
- Cuối cùng, nhân số lần dịch sang phải với số lần dịch xuống, rồi nhân đôi lên
$$2(n-1)(n-2)$$

Ngoài ra, một hình chữ nhật có hai đường chéo, đồng nghĩa là có 2 cách cho hai con mã ăn nhau trong mỗi trường hợp. Vậy `số cách đặt hai con mã để chúng ăn nhau` là:
$$4(n-1)(n-2)$$

Sau cùng, mình có được đáp án của bài toán này là:
$$\frac{k^2(k^2-1)}{2} - 4(n-1)(n-2)$$

### Chương trình
```cpp
#include<iostream>

int main(){
	int n;
	std::cin >> n;
	for(long long i = 1; i <= n; i++){
		std::cout << i*i*(i*i - 1)/2 - 4*(i-1)*(i-2) << "\n";
	}
}
```
Cảm ơn bạn vì đã đọc 👀