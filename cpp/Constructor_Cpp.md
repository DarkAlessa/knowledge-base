# Constructor

ถ้าหากคลาสไม่สร้าง constructor คอมไพเลอร์จะสร้าง default constructor ขึ้นมาให้   
```c++
class x {
};
int main() {
  X x;  // ok สามารถสร้าง object ได้
}
```
แต่ถ้าหากเราสร้าง constructor ขึ้นมาเองคอมไพเลอร์จะไม่สร้าง default constructor   
```c++
class X {
public:
  X(const std::string& text) : text(text) {}
private:
  std::string text;
};
int main() {
  X x;                // แจ้งแตือน "note: candidate expects 1 argument, 0 provided"
  X x("Hello World"); // ok สร้าง object ได้
}
```
หากต้องการสร้าง object ที่ไม่มี argument ต้องสร้าง default constructor เอง   
```c++
class X {
public:
  X() = default;  // หรือ X(){}
  X(const std::string& text) : text(text) {}
private:
  std::string text;
};
int main() {
  X x;                // ok สร้าง object ไม่มี argument ได้
  X y("Hello World"); // ok สร้าง object ได้
}
```

