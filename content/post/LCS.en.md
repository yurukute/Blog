---
title: "Longest common subsequence"
date: 2021-02-17T20:36:47+07:00
tags: ["Dynamic Programing", "Backtracking"]
#ShowToc: true
TocOpen: true
mermaid: true
---
## Bài toán
Source: [Codewars](https://www.codewars.com/kata/593ff8b39e1cc4bae9000070)

From [Wikipedia](https://en.wikipedia.org/wiki/Longest_common_subsequence_problem)
>The longest common subsequence (LCS) problem is the problem of finding the longest subsequence common to all sequences in a set of sequences.
>It differs from problems of finding common substrings: unlike substrings, subsequences are not required to occupy consecutive positions within the original sequences.

Write a function lcs that accepts two strings and returns their longest common subsequence as a string. Performance matters.

For example
```cpp
Input:  "ab" "azb"
Output: "ab"
```
## Solution
I solved this kata in 2 steps:
- Find subsequence's length by dynamic programing.
- Using backtracking on dynamic programing table to find the subsequence.
### Step 1:
Give `lcs("aab", "azb")` as a function to find the longest common subsequence. Its process can be understood as:
- Considering the last characters from 2 strings, they're the same: 
    - Lengthening the subsequence by 1.
    - The problem become `lcs("aa", "az")`.
- Considering the new strings, their last characters are different:
    - One by one, remove the last character from each string and find its longest common subsequence.  `max(lcs("a", "az"), lcs("aa", "a")`

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

For `dp[i][j]` is the length of the longest common subsequence from $i$ fist characters of string $x$ $x$ and $j$ first characters of string $y$, that means:
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
My dynamic programing will look like this:
|  |""|a|z|b|
|- |- |-|-|-|
|**""**|0 |0|0|0|
|**a** |0 |1|1|1|
|**a** |0 |1|1|1|
|**b** |0 |1|1|2|
### Bước 2
Noticed that, at the cell which has the upper cell and the left cell contain the smaller value, that cell's row (or column) is represent for the common character. To find the subsequence, i'll start at the last cell (`dp[n][m]`), go back to the upper row (or left column) until i meet the cell like i mentioned above, store the common character, then go to the cell on its left and up (`dp[i-1][j-1]`) to continue.
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
Thank you for reading.