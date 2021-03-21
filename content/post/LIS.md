---
title: "Dãy con tăng đơn điệu dài nhất"
date: 2021-03-09T19:44:16+07:00
tags: ["Quy hoạch động", "Tìm kiếm nhị phân"]
#ShowToc: true
TocOpen: true
---

## Bài toán 
Nguồn: 
- Tiếng Việt:
    - [Codeforces - Bản dễ](https://codeforces.com/group/FLVn1Sc504/contest/274501/problem/F)&nbsp;&nbsp;&nbsp;*Có thể giải với $O(n^2)$*
    - [Codeforces - Bản khó](https://codeforces.com/group/FLVn1Sc504/contest/274501/problem/G)&nbsp;*Cần giải bằng $O(n\log n)$*
- Tiếng Anh:
    - [Leetcode](https://leetcode.com/problems/longest-increasing-subsequence/)

Cho một dãy số nguyên `nums`. Tìm độ dài của dãy con tăng đơn điệu dài nhất.
Biết rằng dãy con tăng đơn điệu là 1 dãy $a_1,..,a_k$ thỏa mãn 
$$
\begin{align}
    &i_1 < i_2 < \dots < i_k,\\\\\\
    &nums[i_1] < nums[i_2] < \dots < nums[i_k]
\end{align}
$$
Ví dụ:
```cpp
Input:  {0,1,0,3,2,3}
Output: 4 //{0,1,2,3}
```
## Lời giải
### Vét cạn
Đây là phương pháp bần nhất có thể nghĩ: liệt kê hết tất cả các dãy con tăng đơn điệu rồi tìm đâu là dãy con lớn nhất để trả về kết quả. Với độ phức tạp là $O(2^n),$ đây cũng là cách không hiệu quả nhất khi $n$ trở nên lớn.
### Quy hoạch động $O(n^2)$
Để giải bài này, mình chia nó thành các bài toán con và tìm từng kết quả của chúng. Chẳng hạn với *`nums` = {0,1,0,3,2,3}*, các bài toán con mà mình cần giải quyết lần lượt là: 

***Tìm dãy con tăng đơn điệu dài nhất của:***\
    [0-0]&nbsp;&nbsp;&nbsp;*{0}*\
    [0-1]&nbsp;&nbsp;&nbsp;*{0, 1}*\
    [0-2]&nbsp;&nbsp;&nbsp;*{0, 1, 0}*\
    [0-3]&nbsp;&nbsp;&nbsp;*{0, 1, 0, 3}*\
    [0-4]&nbsp;&nbsp;&nbsp;*{0, 1, 0, 3, 2}*\
    [0-5]&nbsp;&nbsp;&nbsp;*{0, 1, 0, 3, 2, 3}* $\Rightarrow$ Bài toán đang giải.

Vậy, bảng quy hoạch động `d` của mình sẽ gồm 5 ô mà mỗi ô tượng trưng cho 1 bài toán con. Giả sử mình đã có đáp án cho các ô `0`, `1`, `2` và cần tìm đáp án cho ô thứ `3`, khi đó, mình cần hỏi bản thân *3* câu hỏi:
- Liệu mình có thể thêm $nums[3]$ vào dãy con đã tìm ở $d[0]$ hay không?
- Liệu mình có thể thêm $nums[3]$ vào dãy con đã tìm ở $d[1]$ hay không?
- Liệu mình có thể thêm $nums[3]$ vào dãy con đã tìm ở $d[2]$ hay không?

Và nếu câu trà lời là `có` (tức là $nums[3] > nums[i]$), mình sẽ lựa chọn xem có `nên` hay `không nên` việc thêm $nums[3]$ vào nếu đáp án ở ô $nums[3]$ hiện tại **bé hơn** / **lớn hơn** cái tìm được.

Khái quát hơn:
$$d[i] = \max_{\substack{j = 0 \dots i-1 \\\\\\ nums[j] < nums[i]}} \left(d[j] + 1\right)$$

Cơ sở của `d` sẽ là 1 vì với một dãy chỉ có 1 phần tử, 1 chính là đáp áp, việc của mình là làm tăng đáp án đó lên khi duyệt qua nó.
|i|0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|$nums$|0|1|0|3|2|3|
|$d$|1|1|1|1|1|1|

Vậy lúc này, công thức sẽ trở thành $$d[i] = \max\left(1, \max_{\substack{j = 0 \dots i-1 \\\\\\ nums[j] < nums[i]}} \left(d[j] + 1\right)\right)$$

Cuối cùng, đáp án chính là ô mang giá trị lớn nhất trong `d`.
```cpp
int lis(std::vector<int>& nums){
    int n = nums.size(), max = 1;
    std::vector<int> lis(n,1);
    for(int i = 1; i < n; i++){
        for(int j = 0; j < i; j++)
            if(nums[j] < nums[i] && lis[i] < lis[j]+1)
                lis[i] = lis[j] + 1;
        if(lis[i] > max)
            max = lis[i];
        }
    return max;
}
```
|i|0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|$nums$|0|1|0|3|2|3|
|$d$|1|2|1|3|3|**4** $\Rightarrow$ đáp án|
### Quy hoạch động và tìm kiếm nhị phân $O(n\log n)$
Mình sẽ vẫn sử dùng bảng quy hoạch động `d` nhưng để lưu lại phần tử kết thúc của dãy con có độ dài là $i$. 

Cơ sở quy hoạch động là $d[0] = -\infty$ trong khi các ô còn lại $d[i] = \infty$ \
Vậy độ dài của dãy con tăng đơn điệu dài nhất sẽ là $l$ với $l$ lớn nhất mà $d[l] < \infty$
```cpp
int lis(std::vector<int>& nums) {
    int n = nums.size(), max = 1;
    const int INF = 1e9;
    std::vector<int> d(n+1, INF);
    d[0] = -INF;
    for (int i = 0; i < n; i++)
        for (int j = 1; j <= n; j++) {
            if (d[j-1] < nums[i] && nums[i] < d[j])
                d[j] = nums[i];
        }
    for (int i = 0; i <= n; i++)
        if (d[i] < INF) max = i;
    return max;
}
```
|i|0|1|2|3|4|5|
|-|-|-|-|-|-|-|
|$nums$|0|1|0|3|2|3|
|$d$|$-\infty$|0|1|2|3|$\infty$|

Mình thấy được rằng:
- `d` luôn được sắp xếp tăng dần
- phần tử $nums[i]$ sẽ chỉ cập nhật nhiều nhất một giá trị $d[j]$

Vì vậy, mình có thể tìm phần tử trong `d` bằng *Tìm kiếm nhị phân* O($\log n$). Trên thực tế, mình chỉ đơn giản là tìm phần tử đầu tiên lớn hơn $nums[i]$ và cập nhật nó như cách thực hiện bên trên.
```cpp
int lis(std::vector<int>& nums) {
    int n = nums.size(), max = 1;
    const int INF = 1e9;
    std::vector<int> d(n+1, INF);
    d[0] = -INF;
    for (int i = 0; i < n; i++){
        int j = upper_bound(d.begin(), d.end(), a[i]) - d.begin();
        if (d[j-1] < nums[i] && nums[i] < d[j])
            d[j] = nums[i];
    }
    for (int i = 0; i <= n; i++)
        if (d[i] < INF) max = i;
    return max;
```
Cảm ơn bạn vì đã đọc.