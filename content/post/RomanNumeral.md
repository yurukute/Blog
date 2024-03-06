---
title: "Chuyển đổi số la mã"
date: 2021-07-25T18:51:05+07:00
tags: ["Chuỗi"]
#ShowToc: true
TocOpen: true
---

## Bài toán
Nguồn: [Codewars](https://www.codewars.com/kata/51b66044bce5799a7f000003)

### Đọc hiểu
Cho lớp `RomanNumerals` để chuyển đổi các chữ số la mã về số nguyên và ngược lại. 
```cpp
#include <string>
#include <vector>

class RomanHelper{
public:
    std::string to_roman(unsigned int n){

    }

    int from_roman(std::string rn){

    }
} RomanNumerals;
```
Việc chuyển đổi phải tuân theo nguyên tắc sau:
- Chữ số La Mã hiện đại được viết bằng cách thể hiện từng chữ số riêng biệt bắt đầu bằng chữ số tận cùng bên trái và bỏ qua bất kỳ chữ số nào có giá trị bằng 0.
    - 1990 được viết là 1000 = M, 900 = CM, 90 = XC; => MCMXC.
    - 2008 được viết là 2000 = MM, 8 = VIII; => MMVIII. 
    - 1666 cũng tương tự: MDCLXVI.
    
    Ví dụ:
    ```
    RomanNumerals.toRoman(1000); // Trả về: 'M'
    RomanNumerals.fromRoman('M'); // Trả về: 1000
    ```
- Bảng kí tự:
    |Kí tự  |I  |V  |X  |L  |C  |D  |M   |
    |:-----:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|
    |Chữ số |1  |5  |10 |50 |100|500|1000|

## Lời giải
### Chuyển số nguyên sang số la mã
Ý tưởng là mình sẽ chuyển từng chữ số theo từng hàng: nghìn, trăm, chục, đơn vị. Giả sử, nếu số cần chuyển lớn hơn 1000 thì ta trừ đi 1000 và thêm kí tự `M` vào chuỗi kết quả. Cứ trừ đến khi nó không được nữa thì chuyển đến hàng trăm. Tính cả các số được biểu diễn đặc biệt như 4 (IV) hay 9 (IX) thì có tất cả 13 trường hợp: 
```cpp
std::string to_roman(unsigned int n){
    std::string res = "";
    std::string rnums[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
    int nums[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1}; 
    for(int  i = 0; i < 13; i ++){
        while (n >= nums[i]){
            res.append(rnums[i]);
            n  -= nums[i];
        }
    }
    return res;
}
```
### Chuyển số la mã sang số nguyên
Như đã biết, mỗi kí tự số la mã đại diện cho một số nguyên, nên trước hết mình sẽ đưa từng kí tự về số nguyên mà nó đại diện.
```cpp
int from_roman(std::string rn){
    int from_roman(std::string rn){
    std::vector<int> a;
    for(char i : rn){
        switch(i){
            case 'I': a.push_back(1);    break;
            case 'V': a.push_back(5);    break;
            case 'X': a.push_back(10);   break;
            case 'L': a.push_back(50);   break;
            case 'C': a.push_back(100);  break;
            case 'D': a.push_back(500);  break;
            case 'M': a.push_back(1000); break;
        }
    }
    ...
}
```
Mục đích của việc này là để tiện cho việc so sánh. Để ý rằng, nếu chữ số phía trước lớn hơn hoặc bằng chữ số phía sau thì cộng thêm vào, chẳng hạn: VI = V + I = 6. Ngược lại, nếu chữ số phía trước nhỏ hơn thì trừ đi. Từ đây, mình sẽ so sánh từng cặp số liên tiếp trong mảng a rồi cộng trừ các kiểu như trên để cho ra kết quả cuối cùng. Vì phần tử cuối cùng không có phần tử phía sau để so sánh nên sẽ được cộng vào trước.
```cpp
int from_roman(std::string rn){
    ...
    int res = a.back(), size = a.size();
    for(int  i = 0; i < size-1; i++)
        if(a[i] >= a[i+1])
            res += a[i];
        else res -= a[i];
    return res;
}
```
Cảm ơn bạn vì đã đọc.