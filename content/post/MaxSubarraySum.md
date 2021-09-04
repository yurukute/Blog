---
title: "Đoạn Con Có Tổng Lớn Nhất"
date: 2021-01-17T20:45:26+07:00
draft: false
tags: ["Quy hoạch động"]
---

## Bài toán: 
Nguồn: [Codewars.com](https://www.codewars.com/kata/54521e9ec8e60bc4de000d6c)

Cho một dãy gồm n số nguyên $a_1, a_2,..., a_n$. Hãy tìm một đoạn con *(dãy gồm các phần tử liên tiếp nhau)* có tổng lớn nhất.
```
Input:  {-2, 1, -3, 4, -1, 2, 1, -5, 4}
Output: 6 //vì đoạn con có tổng lớn nhất là {4, -1, 2, 1}
```
Trường hợp đơn giản nhất là mảng chỉ có số dương, khi đó kết quả chính là tổng tất cả các số trong mảng. Nếu mảng chỉ có số âm, trả về $0$

Một mảng rỗng sẽ được xem như có tổng lớn nhất là $0$. Lưu ý rằng mảng rỗng cũng là mảng con/đoạn con hợp lệ.
## Lời giải
### Cách 1: Vét cạn 
Đây là cách dễ tiếp cận nhất với 2 bước:
1. Tìm tổng **từng** đoạn con {-2}, {1}, {-2, 1} ... 
2. So sánh các tổng để tìm ra kết quả lớn nhất.

Tuy nhiên, với độ phức tạp là $O(n^3)$ cách này sẽ không giải quyết được vấn đề khi nó mang **3 nhược điểm** rất lớn: 
1. Làm chậm quá trình xử lí do phải tính quá nhiều.
2. Sẽ tồn tại những phép tính không cần thiết.
3. Các hành động lặp đi lặp lại.

Chính vì vậy, mình cần một giải pháp nhanh và tốt hơn!
### Cách 2:
- mình gọi $f[i]$ là tổng $i$ phần tử đầu tiên, $f[i]$ được tính như sau:
    - $f[0] = 0$
    - $f[i] = f[i-1] + a[i]$
- Như vậy, tổng các phần tử từ $i$ đến $j$ là: $f[j] – f[i-1]$

Với cách này, mình sẽ tính được $f[i]$ mà không phải lặp lại việc tính toán vì mình đã tính trước ở $f[i-1]$ => Không mắc nhược điểm thứ 3 của cách 1 (UwU). Vậy là giảm độ phức tạp xuống còn $O(n^2)$. Nhưng mà, bài toán này coàn có một cách giải với độ phức tạp $O(n)$ OwO)/
### Cách 3: 
Với ý tưởng của cách 2, mình thêm một biến `sum` để lưu tổng đang tính. Khi duyệt mảng, mình sẽ đứng trước 2 lựa chọn: Thêm phần tử đang xét vào đoạn ***hay*** hắt đầu một đoạn con mới. Như vậy mình cần so sánh xem tổng hiện tại (`sum`) cộng thêm phần tử `arr[i]` đang xét liệu có lớn hơn phần tử đó hay không. Nói cách khác mình lấy `max(sum + a[i], a[i])` -> Đây cũng chính là kết quả cần tìm.
```cpp
#include <vector>

using namespace std;

int maxSequence(const vector<int>& arr){
    int max=0, sum=0, size=arr.size();
    for(int i=0; i<size; i++){
        sum+=arr[i];
        if(arr[i]>sum)
            sum=arr[i];
        if(sum>max) max=sum;
    }
    return max;
}
```
Cảm ơn bạn vì đã đọc.