---
title: "Số xu cần ít nhất để đổi một số tiền"
date: 2021-02-08T15:29:10+07:00
tags: ["Quy hoạch động"]
#ShowToc: true
TocOpen: true
mermaid: true
---
## Bài toán
Nguồn: [Leetcode](https://leetcode.com/problems/coin-change/)
### Đọc hiểu
Bạn có một lượng đồng xu có các mệnh giá khác nhau và tổng số tiền `amount`. Nhiệm vụ của bạn là tìm số xu ít cần **ít nhất** để tạo nên số tiền đó (không giới hạn số đồng xu). Nếu số tiền đó không thể tạo thành thì trả về `-1`.

Ví dụ:
```cpp
Input:  coins = {1,2,5}, amount = 11
Output: 3 //2 đồng 5 và 1 đồng 1
```
```cpp
Input:  coins = {2}, amount = 3
Output: -1 
```
## Lời giải
### Cách 1: Vét cạn bằng đệ qui
Lấy ví dụ như trên, mình có *amount* là 11. Giả sử đồng 5 là đồng cuối cùng được chọn vậy số xu cần tìm sẽ là số xu cần ít nhất để đổi số tiền còn lại cộng thêm 1. Nói cách khác,
$$ coinChange(11) = coinChange(11 - 5) + 1$$
Khi đó, bài toán trở thành tìm ***số xu cần ít nhất để đổi số tiền còn lại***. Đây là một bài toán con. Tương tự với đồng 1 và đồng 2, các bài toán con có thể được thể hiện theo sơ đồ sau
{{<mermaid>}}
graph TD;
    A((11))-->B((10))
    A-->C((9))
    A-->D((6))
    B-->E((9))
    B-->F((8))
    B-->G((5))
    C-->H((8))
    C-->I((7))
    C-->J((4))
    D-->K((5))
    D-->L((4))
    D-->M((1))
{{</mermaid>}}
Mình chỉ vẽ một phần của sơ đồ nhưng có thể thấy, khi xét từng trường hợp như vầy, mình sẽ phải lặp lại việc tính các bài toán con giống nhau (như có 2 số 9 nè, rồi 2 số 8,...). Chính vì vậy, tụi mình cần một giải pháp thông minh hơn, đó là quy hoạch động.
### Cách 2: Quy hoạch động
Cũng với ý tưởng trên, lần này mình có một mảng như sau, gọi là `dp` nhé:
|0|1|2|3|4|5|6|7|8|9|10|11|
|-|-|-|-|-|-|-|-|-|-|-|-|-|
|||||||||||||

Ô thứ $i$ của mảng lúc này là bài toán con: ***số xu cần ít nhất để đổi số tiền $i$***. Như vậy, đáp án của ô thứ *amount* chính là đáp án của bài toán. Và tất nhiên, đáp án đấy sẽ được tính từ các ô trước đó nên mình có thể tận dụng các đáp số đã tính sẵn mà không cần tính lại như cách trên ^w^. 

Để bắt đầu, mình cần điền trước vào mảng như thế này:
|0|1|2|3|4|5|6|7|8|9|10|11|
|-|-|-|-|-|-|-|-|-|-|-|-|-|
|0|12|12|12|12|12|12|12|12|12|12|12|

Với mỗi ô, mình sẽ xét từng xu một xem nó có lớn hơn số tiền mình đang xét hay không? Nếu không có nghĩa là mình có thể dùng đồng xu và đó giá trị của ô đó sẽ là $$dp[i] = min(dp[i-coin] + 1,dp[i])$$
Giả sử $i = 1$, vậy $dp[1] = min(0+1, 12)$. Tính lần lượt như vậy, cuối cùng mảng của mình trở thành:
|0|1|2|3|4|5|6|7|8|9|10|11|
|-|-|-|-|-|-|-|-|-|-|-|-|-|
|0|1|1|2|2|1|2|2|3|3|2|3|

Ô 11 có giá trị bằng 3, cũng tức là đáp án của bài toán ví dụ. Từ đấy, mình có đoạn code thế này:
```cpp
vector<int> dp(amount+1, amount+1);
dp[0] = 0;
for(int i = 1; i <= amount; i++)
    for(int coin : coins)
        if(coin <= i && dp[i - coin] + 1 < dp[i])
            dp[i] = dp[i - coin] + 1;
return dp[amount];
```
Ý tưởng là vậy. Nhưng thực tế là nạp cái code này lên sẽ `wrong answer` ngay, vì cái tội đọc không kỹ đề >D<. Trên đề có bảo rằng 
>Nếu số tiền đó không thể tạo thành thì trả về `-1`.

Vậy đấy, nên là cần phải chỉnh dòng `return` lại 1 tí mới qua được UwU. 
```cpp
return (dp[amount] <= amount ? dp[amount] : -1);
```
Cảm ơn bạn vì đã đọc.
## Bài viết liên quan
[Tổng số cách đổi xu](post/CountChange.md)

[Nhập môn quy hoạch động (VNOI)](https://vnoi.info/wiki/translate/topcoder/dynamic-programming.md)