---
title: "Xâu con chung đài nhất"
date: 2021-02-17T20:36:47+07:00
tags: ["Quy hoạch động", "Quay lui"]
#ShowToc: true
TocOpen: true
mermaid: true
---
## Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/593ff8b39e1cc4bae9000070)
### Đọc hiểu
Theo [Wikipedia](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem):
> Bài toán xâu con chung dài nhất là bài toán tìm một xâu có độ dài lớn nhất và là xâu con của mọi xâu trong  một tập hợp các xâu. Khác với chuỗi con, các phần tử của xâu con không nhất thiết phải liên tiếp nhau.

Với 2 chuỗi được nhập vào, nhiệm vụ của bạn chính là tìm ra xâu con dài nhất của chúng.

Ví dụ:
```cpp
Input:  "ab" "azb"
Output: "ab"
```
## Lời giải
Mình giải bài này theo 2 bước:
- Tìm độ dài của xâu con bằng quy hoạch động.
- Sừ dụng bảng quy hoạch động để quay lui tìm xâu con.
### Bước 1:
Cho `lcs("aab", "azb")` là hàm tìm xâu con dài nhất (bài toán lớn). Quy trình của nó có thể được mô tả như sau:

- Xét thấy kí tự cuối cùng của 2 xâu là giống nhau: 
    - Độ dài của xâu con tăng thêm một.
    - Đưa bài toán về `lcs("aa", "az")`.
- Xét 2 xâu mới, kí tự cuối cùng của 2 xâu không giống nhau:
    - Lần lượt bỏ kí tự cuối ở mỗi xâu và tìm xâu con lớn nhất giữa chúng.  `max(lcs("a", "az"), lcs("aa", "a")`

{{<mermaid>}}
graph TD;
    A("lcs(aab, azb)")-->B((1))
    A-->C("lcs(aa, az)")
    C-->D("max(lcs(a, az), lcs(aa, a)")
    D-->E("lcs(a,az)")
    D-->F("lcs(aa, a)")
    E-->G("max(lcs(a, a), lcs(``, az)")
    F-->H((1))
    F-->I("lcs(a, ``)")
    G-->J("lcs(a, a)")-->L((1))
    G-->K("lcs(``, az)")-->M((0))
    I-->N((0))
{{</mermaid>}}

Gọi `dp[i][j]` là độ dài lớn nhất của xâu con từ $i$ phần tử đầu tiên của xâu $x$ và $j$ phần tử đầu tiên của xâu $y$, như vậy:
$$dp[i][j]=
\begin{cases}
    1 + dp[i-1][j-1]            & x[i] = y[j]\\\\
    max(dp[i-1][j],\ dp[i][j-1]) & x[i] \ne y[j]
\end{cases}
$$
```cpp
int n = x.length(), m = y.length();
vector<vector<int>> dp(n+1, vector(m+1, 0));
for(int i = 1; i <= n; i++)
    for(int j = 1; j <= m; j++)
        if(x[i-1] == y[j-1])
            dp[i][j] = 1 + dp[i-1][j-1];
        else
            dp[i][j] = (dp[i-1][j] > dp[i][j-1] ? dp[i-1][j] : dp[i][j-1]);
```
Bảng quy hoạch động của mình sẽ trông như thế này
|  |""|a|z|b|
|- |- |-|-|-|
|**""**|0 |0|0|0|
|**a** |0 |1|1|1|
|**a** |0 |1|1|1|
|**b** |0 |1|1|2|
### Bước 2:
Để ý rằng, tại những ô mà ô phía trên và ô bên trái của nó có giá trị nhỏ hơn thì cột (hoặc hàng) của ô đó đại diện cho kí tự chung. Để tìm được xâu con chung, mình bắt từ ô cuối cùng (`dp[n][m]`), lùi dần lên (hoặc sang trái) cho đến khi gặp ô như mình đã nói, lưu lại kí tự chung, sau đó nhảy sang ô ở hàng trên và cột bên trái của nó (`dp[i-1][j-1]`) để dò tiếp.
```cpp
string res = "";
while(dp[n][m] != 0){
    while(dp[n][m] == dp[n-1][m]){
        n--;
    }
    while(dp[n][m] == dp[n][m-1]){
        m--;
    }
    res.insert(res.begin(), x[n-1]);
    n--;
    m--;
}
return res;
```
Cảm ơn bạn vì đã đọc.