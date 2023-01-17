---
title: "Cộng 2 số lớn"
date: 2022-06-07T20:43:34+07:00
tags: ["Số lớn"]
#ShowToc: true
TocOpen: true
---

# Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/525f4206b73515bffb000b21)

## Đọc hiểu
Viết chương trình trả về tổng của 2 số nguyên. Đầu vào là 2 số được biểu diễn dưới dạng chuỗi và kết quả trả về cũng lả chuỗi.

Ví dụ
```cpp
add("123", "321"); -> "444"
add("11", "99");   -> "110"
```
## Lời giải
Trong số học, phép cộng có thể được thực hiện bằng cách sắp các số hạng theo chiều dọc và cộng lần lượt từng cột bắt đầu từ cột đơn vị (ngoài cùng bên phải). 
$$
\\begin{aligned}
    105 \\\\
    +\\quad6\\\\
    \\hline
     111\\\\
\\end{aligned}
$$
Để mô phỏng các bước thực hiện, hai chuỗi số hạng được duyệt song song từ vị trí cuối, lần lượt cộng giá trị nguyên của chữ số tại vị trí đó và thêm vào chuỗi kết quả. Do hai chuỗi có độ dài khác nhau và được căn phải nên vị trí của chữ số được tính bằng `length - i` với `i` từ `1` đến `length`.


Khởi tạo biến `carry = 0` để lưu số "nhớ" của phép toán, là số hàng chục khi tổng hai số ở cột đang tính vượt quá `9`. Phần số nhớ sẽ được cộng vào tổng của hai chữ số ở cột tiếp theo hoặc chuỗi kết quả sau khi tính toán xong các cột.
```cpp
#include <string>

std::string add(const std::string& a, const std::string& b) {
    std::string res = "";
    int len_a = a.length(), len_b = b.length(), n = std::max(len_a, len_b);
    int carry = 0;

    for(int i = 1; i <= n; i++){
        if(i <= lenA)
            carry += a[lenA - i] - '0';
        if(i <= lenB)
            carry += b[lenB - i] - '0';
        res.insert(res.begin(), carry % 10 + '0');
        carry /= 10;
    }
    res.insert(res.begin(), carry + '0');

    while(res.length() > 1 && res.front() == '0')
        res.erase(res.begin());
    return res;
}
```
Cảm ơn bạn vì đã đọc.
