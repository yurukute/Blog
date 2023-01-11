---
title: "Thang máy"
date: 2022-06-06T00:45:02+07:00
tags: []
#ShowToc: true
TocOpen: true
---

# Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/58905bfa1decb981da00009e)
## Đọc hiểu
Một tòa nhà nhiều tầng có thang máy trong đó. Ở mỗi tầng có một hàng người chờ được di chuyển đến tầng họ muốn,  được thể hiện bằng một số.

Nhiệm vụ: Đưa tất cả mọi người đến các tầng mà họ muốn đến trong khi tuân thủ các quy luật bên dưới.
```text
    TRƯỚC (Mọi người trong hàng đợi)             SAU (Mọi người tại đích đến của họ)
                   +--+                                          +--+ 
  /----------------|  |----------------\        /----------------|  |----------------\
10|                |  | 1,4,3,2        |      10|             10 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 9|                |  | 1,10,2         |       9|                |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 8|                |  |                |       8|                |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 7|                |  | 3,6,4,5,6      |       7|                |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 6|                |  |                |       6|          6,6,6 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 5|                |  |                |       5|            5,5 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 4|                |  | 0,0,0          |       4|          4,4,4 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 3|                |  |                |       3|            3,3 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 2|                |  | 4              |       2|          2,2,2 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 1|                |  | 6,5,2          |       1|            1,1 |  |                |
  |----------------|  |----------------|        |----------------|  |----------------|
 G|                |  |                |       G|          0,0,0 |  |                |
  |====================================|        |====================================|
```
Trả về danh sách tất cả các tầng mà thang máy đã dừng lại (theo thứ tự dừng).

LƯU Ý: Thang máy luôn bắt đầu ở tầng trệt (và mọi người đang chờ ở tầng trệt có thể vào ngay lập tức).

```cpp
queues = { {}, {}, {5,5,5}, {}, {}, {}, {} };
capacity = 5;
result = {0, 2, 5, 0};
```
### Quy luật
#### Quy luật của thang máy
Thang máy chỉ đi lên hoặc đi xuống!

Mỗi tầng có cả nút UP và DOWN (ngoại trừ tầng trên cùng và tầng trệt chỉ có nút DOWN hoặc UP tương ứng).

Thang máy không bao giờ thay đổi hướng cho đến khi không còn người nào muốn đi lên/xuống theo hướng nó đang di chuyển.

Khi trống, thang máy sẽ cố gắng trở nên thông minh. Ví dụ,
- Nếu nó đang đi lên thì nó có thể tiếp tục đi lên để gom những người ở tầng cao nhất muốn đi xuống.
- Nếu nó đang đi xuống thì nó có thể tiếp tục đi xuống để gom những người ở tầng thấp nhất muốn đi lên.

Thang máy có một sức chứa tối đa.

Khi được gọi, thang máy vẫn sẽ dừng ngay cả khi nó đã đầy, không ai khác có thể vào thang máy trừ khi có người muốn xuống!

Nếu thang máy trống và không có người chờ, thì thang máy sẽ quay trở lại tầng trệt.

#### Quy luật của hàng người
Mọi người ở trong "hàng đợi" đại diện cho thứ tự đến của họ để chờ thang máy.

Tất cả mọi người đều có thể nhấn nút UP/DOWN.

Chỉ những người đi cùng hướng mới có thể vào thang máy.

Vào theo thứ tự "hàng đợi", những người không vào cũng không chặn những người đứng sau họ muốn vào.

Nếu một người không thể vào một thang máy đã đầy, họ sẽ nhấn lại nút UP/DOWN sau khi đã khởi hành mà không có họ.

# Lời giải
Thang máy chỉ quay lại tầng trệt chỉ khi `thang máy trống` và `không có người chờ`. Vì thế, cho đến thời điểm đó, thang máy sẽ lặp đi lặp lại chu kỳ như sau:
1. Đi lên: Vì `thang máy luôn bắt đầu ở tầng trệt` nên điều đầu tiên nó làm là đi lên các tầng trên. 
2. Đi xuống: Xảy ra sau khi thang đã lên đến tầng trên cùng

Tại mỗi tầng $i$ đi qua:
- Trả những người trong thang máy nếu $i$ là tầng họ muốn xuống.
```cpp
bool remove_people(std::vector<int> *lift, int floor){
    bool stop = false;
    for (auto person = lift->begin(); person != lift->end(); person++){
        if (*person == floor){
            lift->erase(person--);
            stop = true;
        }     
    }
    return stop;
}
```
- Đưa người vào thang máy nếu số tầng họ muốn đến thuận với chiều đi chuyển của thang máy cho đến khi hết người hoặc thang máy đầy.
```cpp
bool add_people(std::vector<int> *lift, std::vector<int> *people, int floor, int capacity, int direction){
    bool stop = false;
    for(auto person = people->begin(); person != people->end(); person++){
        if((*person > floor && direction == UP) || (*person < floor && direction == DOWN)){
            stop = true;
            if(int(lift->size()) < capacity){
                lift->push_back(*person);
                people->erase(person--);
            }
        }
    }
    return stop;
}
```
- Nếu một trong hai hành động trên được thực hiện, lưu $i$ vào mảng kết quả.
```cpp
#include <vector>

#define UP 1
#define DOWN 0

bool remove_people(std::vector<int> *lift, int floor){...}

bool add_people(std::vector<int> *lift, std::vector<int> *people, int floor, int capacity, int direction){...}

std::vector<int> the_lift(std::vector<std::vector<int>> queues, int capacity) {
    std::vector<int> res = {0}, lift;
    int top = queues.size();
    bool unfinish = true;
    while(unfinish){
        unfinish = false;
        for(int floor = 0; floor < top; floor++){
            bool stop = remove_people(&lift, floor);
            stop = add_people(&lift, &queues[floor], floor, capacity, UP) || stop;
            if(!queues[floor].empty())
                unfinish = true;
            if(stop && res.back() != floor) res.push_back(floor);
        }
        for(int floor = top-1; floor >= 0; floor--){
            bool stop = remove_people(&lift, floor);
            stop = add_people(&lift, &queues[floor], floor, capacity, DOWN) || stop;
            if(!queues[floor].empty())
                unfinish = true;
            if(stop && res.back() != floor) res.push_back(floor);
        }
    }
    if(res.back()) res.push_back(0);
    return res;
}
```
