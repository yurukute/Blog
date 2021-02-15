---
title: "Tổng số cách đổi xu"
date: 2021-02-09T13:12:26+07:00
tags: ["Quy hoạch động"]
#ShowToc: true
TocOpen: true
---
## Bài toán
Nguồn:  [Leetcode](https://leetcode.com/problems/coin-change-2/),
        [Codewars](https://www.codewars.com/kata/541af676b589989aed0009e7)
### Đọc hiểu:
Bạn có một lượng đồng xu với các mệnh giá khác nhau và một tổng số tiền `amount`. Nhiệm vụ của bạn là tính xem có bao nhiêu cách khác nhau để đổi được số tiền với số xu đã cho. Số lượng xu không giới hạn.

Ví dụ:
```cpp
Input:  amount = 5, coins = {1,2,5}
Output: 4 //{5, 2+2+1, 2+1+1+1, 1+1+1+1+1}
```
## Lời giải
Với bài này mình sẽ lần lượt đi qua các giai đoạn của nó mà ở mỗi gia đoạn số xu của mình sẽ thay đổi dẫn đến sự thay đổi của kết quả bài toán. Mình có bảng quy hoạch động (`ways`) sau:
| |0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|{}|||||||
|{**1**}|||||||
|{1,**2**}|||||||
|{1,2,**5**}|||||||

Mỗi một ô trên bảng là một bài toán con và chúng đều có sự liên quan tới nhau. Giả sử mình ở ô $ways[1][1]$, khi đấy mình sẽ tìm *tổng số cách đổi xu* của số tiền $1$ với các xu {1}; nếu mình ở ô $ways[2][3]$ thì mình sẽ tìm *tổng số cách đổi xu* của số tiền $3$ với các xu {1,2}. Tương tự như vậy, giá trị của ô $ways[i][j]$ sẽ là đáp án của bài toán *tổng số cách đổi xu* của số tiền $j$ với các xu ở hàng $i$. 

Bênh cạnh đó, từng hàng $i$ của bảng lại đại diện cho sự xuất hiện *thêm* của một đồng xu (được in đậm). Như vậy, hàng $i$ sẽ trả lời cho câu hỏi *Liệu mình có dùng đồng xu thứ $i$ không?*

Rõ ràng, mình sẽ không dùng đồng xu thứ $i$ nếu $coins[i-1] > j$, ngược lại thì có thể. Vì thế:
$$ways[i][j]=
    \begin{cases} 
        ways[i-1][j],                           & coins[i-1] > j \\\\
        ways[i-1][j] + ways[i][j-coins[i-1]],   & coins[i-1] \leq j 
    \end{cases}
$$
Giải thích một chút ở công thức này nha. Khi mình không dùng đồng xu đang xét thì bài toán quay ngược lại bài toán con ở hàng trên, hàng không có sự xuất hiện của $coins[i-1]$ nên ta chỉ cần lấy kết quả đó. Nhưng nếu dùng thì số tiền lúc này chỉ còn $j - coins[i-1]$ nên mình sẽ truy hồi về ô $ways[i][j-coins[i-1]]$ để lấy số cách của ô này cộng với trường hợp *[không dùng]*.
### Điền cơ sở
Để bắt đầu thì mình cần điền vào hàng 0 và cột 0 (cơ sở của quy hoạch động) trước. Hàng 0 mang ý nghĩa không có đồng xu nào nên ta sẽ không đổi được bất cứ số tiền nào cả, giá trị của các ô này là `0`. Cột 0 thì mang ý nghĩ dùng các xu được cho đổi ra... **không**, nên các ô của cột sẽ có giá trị là `1` vì cách duy nhất là **không làm gì hết**.

Để thuận tiện khi code, mình sẽ tạo vector 2 chiều với các phần tử là 0. Như vậy, chỉ cần điền cột 0 là được.
```cpp
vector<vector<int>> ways(coins.size() +1, vector<int> (amount + 1, 0));
for(int i = 0; i <= coins.size(); i++)
    ways[i][0] = 1;
```
| |0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|{}|1|0|0|0|0|0|
|{**1**}|1|0|0|0|0|0|
|{1,**2**}|1|0|0|0|0|0|
|{1,2,**5**}|1|0|0|0|0|0|

Việc cần làm tiếp theo chính là điền các ô còn lại. Khi điền hết, ô cuối cùng chính là đáp án của bài toán
```cpp
for(int i = 1; i <= coins.size(); i++)
    for(int j = 1; j <= amount; j++)
        ways[i][j] = ways[i-1][j];
        if(j >= coins[i-1])
            ways[i][j] += ways[i][j - coins[i-1]];
return ways[coins.size()][amount];
```
| |0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|{}|1|0|0|0|0|0|
|{**1**}|1|1|1|1|1|1|
|{1,**2**}|1|1|2|2|3|3|
|{1,2,**5**}|1|1|2|2|3|**4**|
### Cải tiến
Cách làm trên có một nhược điểm là sau khi tính xong dòng thứ $n$ thì việc lưu trữ các dòng từ $1$ đến $n-1$ là dư thừa bởi vì việc tính dòng $n+1$ chỉ phụ thuộc vào dòng $n$. Vì vậy, mình sẽ thay đổi từ mảng 2 chiều thành mảng 1 chiều để lưu kết quả và tự tính chính nó.
```cpp
vector<int> ways(amount + 1);
    ways[0] = 1;
    for(auto coin : coins)
        for(int i = 1; i <= amount; i++)
            if(i >= coin)
                ways[i] += ways[i - coin];
return ways[amount];
```
|0|1|2|3|4|5|
|-|-|-|-|-|-|
|1|1|2|2|3|**4**|

Cảm ơn bạn vì đã đọc.
## Bài viết liên quan
[Tổng số cách phân tích số n]({{< ref "post/ExplosiveSum">}})