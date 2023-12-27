# Union และ Struct
ขนาดของ `union` เท่ากับขนาดของ type ที่ใหญ่ที่สุดใน union
```c++
union U {
    int a;          // 4-byte
    double b;       // 8-byte
    long double c;  // 16-byte
};
// sizeof(U) จะมีค่าเท่ากับ 16-byte
```
```c++
#include <iostream>

struct X { int a, b; };
union U {
    X x;
    struct Y { int c, d, e; };
    struct Z;   // forward declaration
};
struct U::Z { int m, n, o, p; };

int main() {
    std::cout << "sizeof(int)   : " << sizeof(int)  << "-byte" << std::endl;
    std::cout << "sizeof(X)     : " << sizeof(X)    << "-byte" << std::endl;
    std::cout << "sizeof(U::Z)  : " << sizeof(U::Z) << "-byte" << std::endl;
    std::cout << "sizeof(U)     : " << sizeof(U)    << "-byte" << std::endl;
    return 0;
}
```
output   
```
sizeof(int)   : 4-byte
sizeof(X)     : 8-byte
sizeof(U::Z)  : 16-byte
sizeof(U)     : 8-byte
```
จากโค้ดด้านบนจะเห็นว่าขนาดของ U คือ 8-byte ซึ่งเป็นขนาดของตัวแปร x จาก struct X เนื่องจาก struct Y และ struct Z ยังไม่มีการประกาศตัวแปร struct ภายใน U   
```c++
struct X { int a, b; };
union U {
    X x;
    struct Y { int c, d, e; } y;
    struct Z;   // forward declaration
};
struct U::Z { int m, n, o, p; };
```
หลังจาก struct Y ประกาศตัวแปร y ภายใน U ตอนนี้ขนาดของ U จะเท่ากับ 12-byte   
