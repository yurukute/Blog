---
title: "Ứng dụng của các phép thao tác bit"
date: 2021-01-29T20:52:26+07:00
tags: ["Bitwise"]
ShowToc: true
TocOpen: true
---
# Sơ lược
|Phép thao tác trên bit |Kí hiệu|Mô tả  |   |
|          :-:          |  :-:  | :-    | - |
|AND                    |&      |**Cả hai bit** là 1, trả về 1.| Ngược lại trả về 0.|
|OR                     |\      |**Một trong hai** bit là 1, trả về 1.| Ngược lại trả về 0.|
|XOR                    |^      |**Hai bit khác nhau** trả về 1.| Ngược lại trả về 0.|
|NOT                    |~      |Đảo bit, 0 thành 1, 1 thành 0.|
|Dịch trái - Shift left |<\<    |Dịch tất cả các bit sang trái.|
|Dịch phải - Right left |>\>    |Dịch tất cả các bit sang phải.|
# Ứng dụng
## Số nguyên
### Thay đổi bit
```cpp
// Đưa bit thứ n thành 1
x |= (1 << n);

//Đưa bit 0 ngoài cùng thành 1
x |= (x+1);

//Đưa bit thứ n về 0
x &= ~(1 << n);

//Đưa bit 1 ngoài cùng về 0
x &= (x-1);

//Đảo bit thứ n
x ^= (1 << n);

//Lấy ra bit thứ n
(x >> n) & 1;

//Hoán đổi 2 bit cạnh nhau
((x & 10101010) >> 1) | ((x & 01010101) << 1);
```
### Nhân / Chia x với $2^n$ 
```cpp
x << n //nhân
x >> n //chia
```
### Làm tròn lên lũy thừa cơ số 2 
```cpp
x--;
x |= x >> 1;
x |= x >> 2;
x |= x >> 4;
x |= x >> 8;
x |= x >> 16;
x++;
```
### Làm tròn *xuống* lũy thừa cơ số 2 
```cpp
x--;
x |= x >> 1;
x |= x >> 2;
x |= x >> 4;
x |= x >> 8;
x |= x >> 16;
x++;
x = x >> 1; //vẫn là đoạn code trên nhưng thêm dòng này
```
### Lấy phần nguyên của x
```cpp
x >> 0;
6.723 >> 0; //6   
```
### Đảo dấu x
```cpp
x = ~x + 1; // hoặc
x = (x ^ -1) + 1; // x = -x
```
### Giá trị tuyệt đối của x
```cpp
int abs = (x ^ (x >> 31)) - (x >> 31);
```
### Ước lũy thừa cơ số 2 lớn nhất của x 
```cpp
x & (-x);
```
### Giá trị nguyên nhỏ nhất / lớn nhất
```cpp
int minInt = 1 << 31;
int minInt = 1 << -1;

int maxInt = ~(1 << 31);
int maxInt = (1 << 31) - 1;
int maxInt = (1 << -1) - 1;
int maxInt = -1u >> 1;

int maxLong = ((long)1 << 127) -1;
```
### Số nhỏ nhất / lớn nhất giữa 2 số
```cpp
//Cách 1
int min = a & ((a-b) >> 31) | b & (~(a-b) >> 31);
int max = b & ((a-b) >> 31) | a & (~(a-b) >> 31);

//Cách 2
int min = (b ^ (a ^ b) & -(a < b));
int max = (a ^ (a ^ b) & -(a < b));
```
### Trung bình cộng 2 số
```cpp
int avg = (x + y) >> 1;
int avg = ((x ^ y) >> 1) + (x & y);
```
### Hoán đổi giá trị 2 biến
```cpp
//Cách 1
a ^= b;
b ^= a;
a ^= b;

//Cách 2
a = a ^ b ^ (b = a)
```
### Gõ nhanh điều kiện if
Trong một số trường hợp, ta sẽ có đoạn code tựa tựa vầy:
```cpp
if(x == a)
    x = b;
//hoặc
if(x == b)
    x = a;
```
Tương tự, ta đưa đoạn code về dòng sau với phép XOR:
```cpp
x ^= a ^ b;
```
### Kiểm tra
- #### giá trị của bit thứ n 
```cpp
x & (1 << n)
```
- #### x là số lẻ
```cpp
(x & 1) == 1;
```
- #### x là lũy thừa cơ số 2
```cpp
x > 0 && (x & (x - 1)) == 0;
```
- #### a = b 
*(Nhanh hơn 35% trong Javascript)*
```cpp
(a ^ b) == 0;
```
- #### 2 số a, b cùng dấu
```cpp
(a ^ b) >= 0;
```
## Chuỗi
### Chuyển kí tự thường thành hoa
```cpp
c & '_'
```
Nếu kí tự được chuyển đã là kí tự hoa thì không thay đổi. Ví dụ:
```cpp
char c;
c = 'a' & '_';  //c = 'A'
c = 'A' & '_';  //c = 'A'
```
### Chuyển kí tự hoa thành thường
```cpp
c | ' '
```
Nếu kí tự được chuyển đã là kí tự thường thì không thay đổi. Ví dụ:
```cpp
char c;
c = 'A' | ' ';  //c = 'a'
c = 'a' | ' ';  //c = 'a'
```
### Đảo ngược kiểu kí tự
```cpp
c ^ ' '
```
Ví dụ:
```cpp
char c;
c = 'A' ^ ' '; //c = 'a'
c = 'a' ^ ' '; //c = 'A'
```
### Vị trí của kí tự trong bảng chữ cái
```cpp
//Đối với chữ thường
c ^ '`'

//Đối với chữ hoa
c ^ '@'

//Không phân biệt
c & "\x1F"
```
## Nhảm nhí khác
### Dùng XOR để mã hóa một chuỗi
```cpp
#include<iostream>
#define key 5

int main(){
    std::string s;
    std::cin >> s;
    
    //Mã hóa
    for(char &i : s){
        i ^= key;
    }
    std::cout << s + "\n";
    
    //Giải mã
    for(char &i : s){
        i ^= key;
    }
    std::cout << s + "\n";
}
```
```
Input:  Hello
Output: M`iij
        Hello
```
### Đổi hệ màu từ R5G5B5 sang R8G8B8
```
R8 = (R5 << 3) | (R5 >> 2)
G8 = (R5 << 3) | (R5 >> 2)
B8 = (R5 << 3) | (R5 >> 2)
```
Viết xong cái này thì mình cũng sắp lú rồi, cảm ơn vì đã đọc. 