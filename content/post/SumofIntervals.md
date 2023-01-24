---
title: "Tổng các khoảng"
date: 2021-07-27T09:40:22+07:00
tags: ["Số học"]
#ShowToc: true
TocOpen: true
---

## Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/52b7ed099cdc285c300001cd)
### Đọc hiểu
Viết hàm tên `sumIntervals`/`sum_intervals()` nhận vào 1 mảng các cặp số nguyên biểu thị thời điểm đầu và cuối của một khoảng. Nhiệm vụ của mình là tính tổng độ dài của các khoảng ấy. Các khoảng lồng nhau như [1, 4] với [3, 5] thì tính là [1, 5] luôn o.o)/ 

Ví dụ
```cpp
sum_intervals( {
   {1,2},
   {6, 10},
   {11, 15}
} ); // => 9

sum_intervals( {
   {1,4},
   {7, 10},
   {3, 5}
} ); // => 7

sum_intervals( {
   {1,5},
   {10, 20},
   {1, 6},
   {16, 19},
   {5, 11}
} ); // => 19
```
## Lời giải
Để ý rằng, nếu một khoảng có thời điểm đầu **nhỏ hơn** thời điểm cuối của khoảng liền trước thì chúng đang **lồng nhau**. Có hai trường hợp như sau:
- Đầu khoảng này là đuôi khoảng kia. Ví dụ: [1, 4] và [3, 5]
    - Thời điểm cuối của khoảng thứ hai **lớn hơn** thời điểm cuối của khoảng thứ nhất
    - Khoảng sau khi gộp là: [1, 5]
- Khoảng này nằm hoàn toàn trong khoảng kia. Ví dụ: [1, 5] và [2, 4]
    - Thời điểm cuối của khoảng thứ hai **nhỏ hơn** thời điểm cuối của khoảng thứ nhất
    - Khoảng sau khi gộp là khoảng thứ nhất: [1, 5]

Vậy, trước tiên mình cần sort `intervals` từ bé đến lớn theo các thời điểm đầu để không bỏ sót các khoảng lồng nhau nào, dẫn đến kết quả trả về cao hơn kỳ vọng. Dùng một mảng `merged` để lưu các khoảng sau khi gộp, với `merged.back()` đóng vai trò là khoảng liền trước khoảng đang xét. Lúc này, nếu khoảng đang xét có lồng với khoảng trước thì cập nhật thời điểm cuối của khoảng trước đó - `merged.back()` -  cho phù hợp, nếu không thì thêm khoảng đang xét vào `merged`. Sau cùng chỉ cần cộng các khoảng đã gộp là xong.
```cpp
#include <vector>
#include <utility>
#include <algorithm>

int sum_intervals(std::vector<std::pair<int, int>> intervals) {
    int size = intervals.size();
    std::sort(intervals.begin(), intervals.end());
    std::vector<std::pair<int,int>> merged = {{intervals[0].first, inttervals[0].second}};
    for(int i = 1; i < size; i++){
        if(intervals[i].first <= merged.back().second)      //Lồng nhau:
            if (intervals[i].second >= merged.back().second)    // Trường hợp (1)
                merged.back().second = intervals.second; 
            else continue;                                      // Trường hợp (2)
        else merged.push_back(intervals[i]);                // Không lồng nhau
    }
    int res = 0;
    for(const auto &i : merged)
        res += i.second - i.first;   
    return res;
}

```
### Cải tiến 
Gọi `x` là thời điểm cuối của khoảng trước khoảng đang xét (tức là nó chứa giá trị của `merged.back().second` ở trên). Vẫn với hai trường hợp lồng nhau nhưng mình sẽ thay đổi thứ tự của các trường hợp khi xét: 
- Nếu thời điểm cuối của khoảng đang xét **lớn hơn** hoặc bằng x sẽ có 2 khả năng:
    - Nếu thời điểm đầu của nó mà **nhỏ hơn** x thì nó đang bị lồng theo **trường hợp (1)**
    - Nếu thời điểm đầu của nó mà **lớn hơn** x thì nó **không có lồng nhau** với khoảng trước nó. 
    - Dù là khả năng thứ nhất hay thứ hai thì giá trị của x sẽ cần được cập nhật thành thời điểm cuối của khoảng đang xét.
- Ngược lại, nếu nó **nhỏ hơn** x thì nó đang bị lồng theo **trường hợp (2)**. Tức là có thể bỏ qua nó.

Với phương pháp này mình chỉ sử dụng cấu trúc `if` một lần và không cần mảng `merged` khiến code trông gọn hơn.
```cpp
#include <vector>
#include <utility>
#include <algorithm>

int sum_intervals(std::vector<std::pair<int, int>> intervals) {
    std::sort(intervals.begin(), intervals.end());
    int x = intervals[0].first, res = 0;
    for (auto &i : intervals){
        if (i.second >= x){
            res += i.second - (i.first > x ? i.first : x);
            x = i.second;
        }    
    }
    return res;
}
```
Cảm ơn bạn vì đã đọc.