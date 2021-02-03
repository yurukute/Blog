---
title: "Bitwise operators' application"
date: 2021-01-29T20:52:33+07:00
tags: ["Bitwise"]
ShowToc: true
TocOpen: true
---

# Summary
|Name                   |Operator|Description  |   |
|          :-:          |  :-:   | :-    | - |
|AND                    |&       |**Both bits** are 1, return 1.| Otherwise, return 0.|
|OR                     |\       |**One of both** bit is 1, return 1.| Otherwise, return 0.|
|XOR                    |^       |**Two bits are different** return 1.| Otherwise, return 0.|
|NOT                    |~       |Flip bit, 0 becomes 1, 1 becomes 0|
|Shift left             |<\<     |Shifts all the bits to the left|
|Right left             |>\>     |Shifts all the bits to the right|
# Application
## Integer
### Change bit
```cpp
//Set nth bit
x |= (1 << n);

//Set the right-most 0 bit to 1
x |= (x+1);

//Unset nth bit
x &= ~(1 << n);

//Set the right-most 1 bit to 0
x &= (x-1);

//Toggle nth bit
x ^= (1 << n);

//Get the mth bit of n
(x >> n) & 1;

//Swap Adjacent bits
((x & 10101010) >> 1) | ((x & 01010101) << 1);
```
### Multiplication / Division x by $2^n$ 
```cpp
x << n //multiplication
x >> n //division
```
### Round up to the next power of two
```cpp
x--;
x |= x >> 1;
x |= x >> 2;
x |= x >> 4;
x |= x >> 8;
x |= x >> 16;
x++;
```
### Round *down* to the next power of two
```cpp
x--;
x |= x >> 1;
x |= x >> 2;
x |= x >> 4;
x |= x >> 8;
x |= x >> 16;
x++;
x = x >> 1; //the same with the code above but added this line
```
### Floor x
```cpp
x >> 0;
6.723 >> 0; //6   
```
### Flip the sign
```cpp
x = ~x + 1; // or
x = (x ^ -1) + 1; // x = -x
```
### Absolute value of x
```cpp
int abs = (x ^ (x >> 31)) - (x >> 31);
```
### The greatest power of 2 that divides x
```cpp
x & (-x);
```
### Get the minimum / maximum integer (and long)
```cpp
int minInt = 1 << 31;
int minInt = 1 << -1;

int maxInt = ~(1 << 31);
int maxInt = (1 << 31) - 1;
int maxInt = (1 << -1) - 1;
int maxInt = -1u >> 1;

int maxLong = ((long)1 << 127) -1;
```
### The min / max of two values
```cpp
//First way
int min = a & ((a-b) >> 31) | b & (~(a-b) >> 31);
int max = b & ((a-b) >> 31) | a & (~(a-b) >> 31);

//Second way
int min = (b ^ (a ^ b) & -(a < b));
int max = (a ^ (a ^ b) & -(a < b));
```
### Average
```cpp
int avg = (x + y) >> 1;
int avg = ((x ^ y) >> 1) + (x & y);
```
### Swap 2 variables
```cpp
//First way
a ^= b;
b ^= a;
a ^= b;

//Second way
a = a ^ b ^ (b = a)
```
### Quick conditional assignment
In some cases. you may require some conditional assignment such as
```cpp
if(x == a)
    x = b;
//hoặc
if(x == b)
    x = a;
```
You can use bitwise XOR operator for these type of assignment.
```cpp
x ^= a ^ b;
```
### Check if...
- #### nth bit is set
```cpp
x & (1 << n)
```
- #### x is an odd number
```cpp
(x & 1) == 1;
```
- #### x is a power of 2
```cpp
x > 0 && (x & (x - 1)) == 0;
```
- #### a = b 
*(35% faster in Javascript)*
```cpp
(a ^ b) == 0;
```
- #### both number have the same sign
```cpp
(a ^ b) >= 0;
```
## String
### Convert letter to lowercase or uppercase
```cpp
c & '_'
```
If the converting letter is uppercase, nothing change. Eg:
```cpp
char c;
c = 'a' & '_';  //c = 'A'
c = 'A' & '_';  //c = 'A'
```
### Convert letter to an uppercase or lowercase
```cpp
c | ' '
```
If the converting letter is a lowercase, nothing change. Eg:
```cpp
char c;
c = 'A' | ' ';  //c = 'a'
c = 'a' | ' ';  //c = 'a'
```
### Invert letter's case
```cpp
c ^ ' '
```
For example:
```cpp
char c;
c = 'A' ^ ' '; //c = 'a'
c = 'a' ^ ' '; //c = 'A'
```
### Letter's position in alphabet
```cpp
//For lowercase only
c ^ '`'

//For uppercase only
c ^ '@'

//Letter case is not important
c & "\x1F"
```
## Miscellaneous
### Use XOR for basic encryption and decryption
```cpp
#include<iostream>
#define key 5

int main(){
    std::string s;
    std::cin >> s;
    
    //Mã hóa
    for(char &i : s){
        i ^= key;
    }
    std::cout << s + "\n";
    
    //Giải mã
    for(char &i : s){
        i ^= key;
    }
    std::cout << s + "\n";
}
```
```
Input:  Hello
Output: M`iij
        Hello
```
### Fast color conversion from R5G5B5 to R8G8B8 pixel format using shifts
```
R8 = (R5 << 3) | (R5 >> 2)
G8 = (R5 << 3) | (R5 >> 2)
B8 = (R5 << 3) | (R5 >> 2)
```
I'm gonna lose my mind after finished this post, thanks for reading.