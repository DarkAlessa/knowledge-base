cmake จะทำการ rebuild ใหม่ก็ต่อเมื่อไฟล์ CMakeLists.txt ถูกแก้ไข แต่การใช้ file(GLOB ...) เพื่อสร้างลิสของไฟล์จะทำให้เราไม่ต้องแก้ไขไฟล์ ทุกครั้งที่เราเพิ่มไฟล์เราจะต้องใช้คำสั่ง cmake -S . -B ./path -G "..." ทุกครั้งเพื่อให้ลิสไฟล์อัพเดท

### กรณีใช้  file(GLOB ...)
```cmake
file(GLOB list ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)
add_library(archive ${list})
```
```
1) cmake -S . -B ./path -G "..."	// สร้าง Project Buildsystem
2) cmake --build ./path             // build project
3) เพิ่ม source file ใหม่ในโปรเจค
4) rm -rf ./path 
   cmake -S . -B ./path -G "..."	// สร้าง Project Buildsystem ใหม่เพื่ออัพเดทไฟล์ลิส
5) cmake --build ./path
```
การ build โปรเจคใหม่ทั้งหมดจึงไม่เหมาะโดยเฉพาะโปรเจคขนาดใหญ่

### กรณีที่ไม่ใช้  file(GLOB ...) 
```cmake
add_library(archive 
	a.cpp 
	b.cpp 
	c.cpp
)
```
```
1) cmake -S . -B ./path -G "..."    // สร้าง Project Buildsystem
2) cmake --build ./path             // build project
3) เพิ่ม source file ใหม่ในโปรเจค
4) เพิ่มชื่อไฟล์ใน CMakeLists.txt        // ไฟล์ CMakeLists.txt ถูกอัพเดท
5) cmake --build ./path             
```
จะสังเกตว่าการไม่ใช้ file(GLOB) จะเป็นการบังคับให้เราต้องแก้ CMakeLists.txt ทุกครั้งที่เราเพิ่ม source file เราจึงใช้คำสั่ง cmake --build ./path ได้เลย

### สรุป
file(GLOB ...) จะเหมาะกับลิสที่ไม่ปลี่ยนแปลงจำนวนไฟล์
