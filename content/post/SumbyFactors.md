---
title: "Sum by Factors"
date: 2021-01-18T16:50:30+07:00
tags: ["codewars", "eratosthene"]
---

## Bài toán:
Nguồn: [Codewars.com](https://www.codewars.com/kata/54d496788776e49e6b00052f)
### Đọc hiểu:
Input: Một mảng các số nguyên: $I = [i_1, i_2 ,..., i_n]$

Output: một chuỗi theo dạng ($p_1$ $sum_1$) ($p_2$ $sum_2$) ... ($p_x$ $sum_m$)

Trong đó:
- $p$ : số nguyên tố
- $sum$ : tổng các số trong mảng I chia hết cho p

Ví dụ:
```
I = {12, 15}; //kết quả: "(2 12)(3 27)(5 15)"
```
Như vậy, để giải được bài này ta cần tìm các số nguyên tố từ nhỏ đến lớn, ứng với mỗi số nguyên tố, ta có:
- Nếu số $i_j$ chia hết cho $p$
    - $sum$ += $i_j$
## Liệt kê các số nguyên tố
Các liệt kê nhanh nhất là dùng sàng. Nhưng mà sàng đến bao nhiêu? Chính là sàng đến giá trị tuyệt đối lớn nhất trong số các phần tử của mảng I, gọi là `max`. Vì vậy, việc đầu tiến sau khi nhập mảng vào thì ta tìm `max`. Cách để sàng nguyên tố chắc ai cũng biết, không biết thì [đây](https://vnoi.info/wiki/translate/he/Number-Theory-2.md). Các số mình kiểm tra thì nhiều nhưng chỉ có vài trong số đó là số nguyên tố. Vì vậy, sau khi sàng xong, mình chỉ lưu lại những số nào là số nguyên tố thôi
```cpp
std::vector<int> sieve(int n){
    std::vector<bool> isprime(n+1, 1);
    isprime[0] = isprime[1] = 0;
    for(int i = 2; i*i <= n; i++)
        if(isprime[i]==1)
            for(int j = i*i; j <= n; j += i)
                isprime[j]=0;
    std::vector<int> res;
    for(int i = 0; i < n+1; i++)
        if(isprime[i])
            res.push_back(i);
    return res;
};
```
## Bắt đầu xử lí
```cpp
for(auto i : primes){
    ans += '(';
    int sum = 0;
    for(auto j : lst)
        if(j % i == 0)
            sum += j;
    if(sum)
        ans = ans + to_string(i) + ' ' + to_string(sum);
    else{
        ans.pop_back();
        continue;
    }
    ans += ')';
}
```
Trong đoạn code trên, mình dùng một cái hàm `to_string()`. Đây là hàm mình tự viết. Bởi vì output ở dạng chuỗi nên mình cần phải chuyển `p` và `sum` về dạng chuỗi rồi gắn vào `ans`:
```cpp
    std::string to_string(int n){
    std::string prefix = "", s = ""; //Lưu lại dấu "-" nếu n là số âm
    if(n < 0){
        n *= -1;
        prefix = "-";
    }
    while(n > 0){
        int x = n % 10;
        n /= 10;
        //Đây là phép tách số từ sau ra trước. Vì vậy các số tách ra sau cần phải thêm vào hàng đầu.
        s.insert(s.begin(), x+48); 
    }
    return prefix+s;
};
```
Thế là xong! Ghép lại là có thể submit UwU.
## Tái bút
Thật ra cái code này còn naive lắm. Submit xong sẽ thấy ngay nhiều solution code ngắn và sáng tạo. Mình thì bám váo cấu trúc của kata, tức là không import thêm thư viện nào vào, vì vậy mới phải viết thêm hàm `to_string()`. Nếu không có hàm đó thì code sẽ ngắn lại đáng kể đấy!

