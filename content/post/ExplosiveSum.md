---
title: "Tổng số cách phân tích n"
date: 2021-02-15T18:49:54+07:00
tags: ["Quy hoạch động"]
#ShowToc: true
TocOpen: true
---
## Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/52ec24228a515e620b0005ef)
### Đọc hiểu
**Có bao nhiêu cách phân tích số n thành tổng các số?**

Theo wikipedia:  [https://vi.wikipedia.org/wiki/Phân_hoạch_(lý_thuyết_số)](https://vi.wikipedia.org/wiki/Phân_hoạch_(lý_thuyết_số))
>Trong số học, sự phân tích một số nguyên dương n là cách viết số đó dưới dạng tổng của các số nguyên dương. Hai cách phân tích có các số hạng giống nhau được coi là một cách phân tích.
Ví dụ:
```cpp
Input:  4
Output: 5 //4, 3+1, 2+2, 2+1+1, 1+1+1+1
```
## Lời giải
Bài toàn này có thể dùng phương pháp liệt kê để đếm nhưng mà như vậy rất là chậm khi $n$ là số lớn nên mình sẽ dùng quy hoạch động (Oω<)☆.

Mình có bảng quy hoạch động (`ways`) sau:
||0|1|2|3|4|
|-|-|-|-|-|-|
|0||||||
|1||||||
|2||||||
|3||||||
|4||||||

Như vậy,`ways[i][j]` sẽ là *số cách phân tích số $j$ thành tổng các số nguyên dương $\leq i$*.
Cách phân tích số $j$ có thể chia làm hai loại
- Không chứa số i

    Bài toán lúc này trở thành *số cách phân tích số $i$ thành tổng các số nguyên dương $< i$*, nói cách khác là $\leq i - 1$
- Chứa số i

    Khi đó, nếu bỏ đi $i$ mình sẽ được các cách phân tích số $j - i$

Rõ ràng, các cách phân tích chỉ có thể là loại 1 nếu $i > j$, ngược lại, số cách sẽ là tổng 2 loại trên. Vì thế:
$$ways[i][j]=
    \begin{cases} 
        ways[i-1][j],                   & i > j  \\\\
        ways[i-1][j] + ways[i][j-i],    & i \leq j
    \end{cases}
$$

Để tính được `ways[i][j]` thì mình phải tính trước`ways[i-1][j]` và `ways[i][j-i]` nên ban đầu mình phải điền vào hàng 0, cơ sở của bảng. Vì số cách phân tích số 0 là 1 nên `ways[0][0] = 1`, còn lại tất cả sẽ là 0. Sau đó, dùng công thức truy hồi tìm tất cả các ô, lúc này `ways[n][n]` sẽ là kết quả mình cần tìm.
```cpp
#include<vector>

using ull = unsigned long long;

ull exp_sum(unsigned int n) {
    std::vector<std::vector<ull>> ways(n+1, std::vector<ull> (n+1, 0));
    ways[0][0] = 1;
    for(unsigned int i = 1; i <= n; i++){
        for(unsigned int j = i; j <= n; j++){
            ways[i][j] = ways[i-1][j];
            if(i <= j)
                ways[i][j] += ways[i][j-i];
        }
    }
    return ways[n][n];
}
```
||0|1|2|3|4|
|-|-|-|-|-|-|
|0|1|0|0|0|0|
|1|1|1|1|1|1|
|2|1|1|2|2|3|
|3|1|1|2|3|4|
|4|1|1|2|3|**5**|
### Cải tiến
Có thể thấy, sau khi tính xong dòng thứ $n$ thì việc lưu trữ các dòng từ $1$ đến $n-1$ là dư thừa bởi vì việc tính dòng $n+1$ chỉ phụ thuộc vào dòng $n$. Nên mình sẽ dùng mảng 1 chiểu để lưu trữ và tự tính chính nó.
```cpp
#include<vector>

using ull = unsigned long long;

ull exp_sum(unsigned int n) {
    std::vector<ull> ways(n+1, 0);
    ways[0] = 1;
    for(unsigned int i = 1; i <= n; i++){
        for(unsigned int j = i; j <= n; j++){
            ways[j] += ways[j - i];
        }
    }
    return ways[n];
}
```
|0|1|2|3|4|
|-|-|-|-|-|
|1|1|2|3|**5**|

Cảm ơn bạn vì đã đọc.
## Bài viết liên quan
[Tổng số đổi đồng xu]({{< ref "post/CountChange">}})
