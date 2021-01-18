---
title: "Sum by Factor"
date: 2021-01-18T16:50:30+07:00
draft: false
tags: ["Codewars"]
---

## Bài toán:
[Link trên codewars](https://www.codewars.com/kata/54d496788776e49e6b00052f)
### Đọc hiểu:
Input: một mảng các số $I = {i_1, i_2 ,..., i_n}$\
Output: một chuỗi theo format "$(p_1 sum_1)(p_2 sum_2)...(p_x sum_m)$"\
Trong đó:
- p: số nguyên tố
- sum: tổng các số trong mảng I chia hết cho p

Như vậy, để giải được bài này ta cần tìm các số nguyên tố từ nhỏ đến lớn, ứng với mỗi số nguyên tố, ta có:
- Nếu số $i_j$ chia hết cho p
    - sum += $i_j$
## Liệt kê các số nguyên tố
Các liệt kê nhanh nhất là dùng sàng. Nhưng mà sàng đến bao nhiêu? Chính là sàng đến giá trị tuyệt đối lớn nhất trong số các phần tử của mảng I, gọi là `max`. Vì vậy, việc đầu tiến sau khi nhập mảng vào thì ta tìm `max`
```cpp
int max = 0; //Ở đây chỉ xét trị tuyệt đối nên max không cần phải bằng -infinite 
for(auto i : lst)
    if(abs(i) > max)
        max = abs(i)
```
Cách để sàng nguyên tố chắc ai cũng biết, không biết thì [đây](https://vnoi.info/wiki/translate/he/Number-Theory-2.md).<br>
Các số mình kiểm tra thì nhiều nhưng chỉ có vài trong số đó là số nguyên tố. Vì vậy, sau khi sàng xong, mình chỉ lưu lại những số nào là số nguyên tố thôi
```cpp
...
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
Thật ra cái code này còn naive lắm. Submit xong sẽ thấy ngay nhiều solution code ngắn và sáng tạo. Mình thì bám váo cấu trúc của codewars, tức là không import thêm thư viện nào vào, vì vậy mới phải viết thêm hàm `to_string()`. Nếu không có hàm đó thì code sẽ ngắn lại đáng kể đấy!

