---
title: "Xoắn ốc"
date: 2024-03-05T20:25:59Z
tags: ["Mảng"]
#ShowToc: true
TocOpen: true
---

## Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/521c2db8ddc89b9b7a0000c1)
### Đọc hiểu
Cho một mảng kích thước nxn, trả về các phần tử của mảng từ ngoài vào trong theo chiều kim đồng hồ.
```
array = [[1,2,3],
         [4,5,6],
         [7,8,9]]
snail(array) #=> [1,2,3,6,9,8,7,4,5]
```
Để hiểu rõ hơn, hãy lần lượt xem các số liên tiếp trong mảng tiếp theo:
```
array = [[1,2,3],
         [8,9,4],
         [7,6,5]]
snail(array) #=> [1,2,3,4,5,6,7,8,9]
```
Bức hình sau sẽ thể hiện mọi thức rõ ràng hơn:

{{< svg "static/snail.svg" >}}

CHÚ Ý: Ý tưởng không phải là sắp xếp các phần tử từ bé đến lớn mà là duyệt mảng 2 chiều theo hình xoắn ốc thuận chiều kim đồng hồ.

CHÚ Ý 2: Mảng rỗng 0x0 được biểu diễn là một mảng chỉ chứa một mảng rỗng [[]].

## Lời giải
Mác 4 kyu nhưng bài này lại đơn giản hơn mấy bài khác. Để duyệt được hình xoắn ốc thì mình duyệt lần lượt hàng `0`, cột `n-1`, hàng `n-1`, cột `1`,... từ ngoài vào trong.

Đầu tiên là mình cần khai báo các biến sau nè:
```cpp
std::vector<int> res;
int row_start = 0;                  // Hàng trên ngoài cùng
int col_start = 0;                  // Cột trái ngoài cùng
int row_end = snail_map.size();     // Hàng dưới ngoài cùng
int col_end = row_end;              // Cột phải ngoài cùng
```
Bắt đầu từ hàng trên ngoài cùng, để duyệt hàng thì ta tăng chỉ số cột của hàng đó lên:
```cpp
for (int i = col_start; i < col_end; i++)  
    res.push_back(snail_map[row_start][i]);
```
Tương tự với cột phải thì ta tăng chỉ số hàng. Lưu ý là phải bỏ qua phần tử ở góc phải trên cùng (`snail_map[row_start][col_end-1]`) vì nó đã được duyệt.
```cpp
for (int i = ++row_start; i < row_end; i++) 
    res.push_back(snail_map[i][col_end-1]);
```
Vì là hình xoắn ốc nên khi duyệt hàng dưới phải duyệt theo chiều ngược lại: giảm chỉ số cột thay vì tăng. Và vì phần tử góc phải dưới cùng (`snail_map[row_end-1][col_end-1]`) đã được duyệt, cột ngoài cùng sẽ được bỏ qua.
```cpp
for (i = (--col_end)-1; i >= col_start; --i) 
    res.push_back(snail_map[row_end-1][i]); 
```
Tương tự với cột trái ngoài cùng, giảm chỉ số của hàng để duyệt ngược lên, bỏ qua hàng dưới.
```cpp
for (int i = (--row_end)-1; i >= row_start; --i)
    res.push_back(snail_map[i][col_start]);
```

Các hành động này được lặp đi lặp lại cho đến khi hết mảng, hay nói cách khác là `row_start` và `row_end`, hoặc `col_start` và `col_end`, chạy qua nhau (`_start` $\geq$ `_end`). Ngoài ra, cần phải xét đến trường hợp mảng rỗng.
```cpp
if(snail_map.empty() || snail_map[0].empty())
    return {};
```
Như vậy, lời giải của bài toán đã hoàn thiện.
```cpp
#include<vector>

std::vector<int> snail(const std::vector<std::vector<int>> &snail_map){
    if(snail_map.empty() || snail_map[0].empty())
        return {};

    std::vector<int> res;
    int row_start = 0;                  // Hàng trên ngoài cùng
    int col_start = 0;                  // Cột trái ngoài cùng
    int row_end = snail_map.size()-1;   // Hàng dưới ngoài cùng
    int col_end = row_end;              // Cột phải ngoài cùng

    while (row_start <= row_end && col_start <= col_end){  
        for (int i = col_start; i <= col_end; i++)  
            res.push_back(snail_map[row_start][i]); 
       
        for (int i = ++row_start; i <= row_end; i++) 
            res.push_back(snail_map[i][col_end]); 
        
        for (int i = --col_end; i >= col_start; i--)
            res.push_back(snail_map[row_end][i]); 
        
        for (int i = row_end; i >= row_start; i--)
            res.push_back(snail_map[i][col_start]);
        
        col_start++;
    }
    return res;
};
```
## Vẫn là lời giải
Nếu bạn là người quen nhìn `while` thì lời giải có thể viết lại thế này:
```cpp
    int row = 0, col = 0;
    while (row_start <= row_end && col_start <= col_end) {
        while (col < col_end)   res.push_back(snail_map[row][col++]);  row_start++;
        while (row < row_end)   res.push_back(snail_map[row++][col]);  col_end--;
        while (col > col_start) res.push_back(snail_map[row][col--]);  row_end--;
        while (row > row_start) res.push_back(snail_map[row--][col]);  col_start++;
    }
    res.push_back(snail_map[row][col]); // Bổ sung phần tử cuối
```
Hai biến `row` và `col` được dùng để đánh dấu hàng và cột đang duyệt, hai biến này chỉ chạy từ `0` đến `n-2` và ngược lại. Sở dĩ chỉ chạy đến `n-2` là để ở dòng `while` tiếp theo vẫn có thể tận dụng lại giá trị của `row`, `col` mà không duyệt trùng phần tử, nói cách khác, thay vì bỏ qua phần tử đã duyệt trùng thì giờ mình luôn chừa lại một phần tử cuối hàng/cột mỗi lần duyệt. Vì thế, sau khi kết thúc dòng `while` mình cần bổ sung phần tử cuối cùng vào để ra đáp án hoàn chỉnh.

Cảm ơn bạn vì đã đọc.

---

### *Tái bút*
Hồi bé ngơ ngác thi tin học trẻ cũng gặp một bài xoắn ốc như này, mà nó tên là Thành Cổ Loa cơ. Lạy Chúa chứ lúc đó đọc để chả hiểu gì đâu, nay lướt trúng kata này nhìn lại thì thấy nó đơn giản (hoặc do mình quên cái đề hồi đó rồi (:3 ). Thôi thì đấm kata này coi như mở bát 2024 và ôn kỷ niệm luôn.