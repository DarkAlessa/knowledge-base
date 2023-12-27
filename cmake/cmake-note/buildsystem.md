## Buildsystem
```
                  Object file
╭───────╮         ╭──────╮
│ a.cpp ├─────────┤ a.o  ├───╮
╰───────╯         ╰──────╯   │      Static library
╭───────╮         ╭──────╮   │      ╭───────────╮
│ b.cpp ├─────────┤ b.o  ├───┼──────┤ archive.a ├────╮
╰───────╯         ╰──────╯   │      ╰───────────╯    │        Executable file
╭───────╮         ╭──────╮   │                       │        ╭──────────╮
│ c.cpp ├─────────┤ c.o  ├───╯                       ├────────┤ main.exe │
╰───────╯         ╰──────╯                           │        ╰──────────╯
╭──────────╮      ╭─────────╮                        │    
│ main.cpp ├──────┤ main.o  ├────────────────────────╯
╰──────────╯      ╰─────────╯
```

### Binary Targets
`Target` คือไฟล์ไบนารี่ที่สร้างจาก add_library() และ add_executable() 

### Object Targets
ไลบรารี `OBJECT` คือไฟล์ที่ถูกคอมไพล์จาก source ไฟล์แต่ไม่ใช่ `archive file` หรือเชื่อมโยงไฟล์ object ลงในไลบรารี <br>
`archive file` คือไฟล์คอมพิวเตอร์ที่ประกอบด้วยไฟล์ตั้งแต่หนึ่งไฟล์ขึ้นไปพร้อมกับข้อมูลเมตา ใช้เพื่อรวบรวมไฟล์ข้อมูลหลาย ๆ ไฟล์เข้าด้วยกันเป็นไฟล์เดียวเพื่อการพกพาและการจัดเก็บที่ง่ายขึ้น

```cmake
#syntax
add_library(obj-name OBJECT ...)

#example
add_library(objects OBJECT a.cpp b.cpp c.cpp)
```

### Static Library
static library(.a) คือคอลเลคชั่นของ object file(.o) ซึ่งถือเป็น `archive file` โดยดีฟอลต์ add_library() จะสร้างเป็น static library ถ้าไม่ใส่ `STATIC`
```cmake
#syntax
add_library(archive-name STATIC ...)

#example
add_library(objects OBJECT a.cpp b.cpp c.cpp)
add_library(archive STATIC $<TARGET_OBJECTS:objects>)
```

หรือสร้างได้โดยไม่ต้องสร้าง `OBJECT` ก่อนเพราะ add_library() จะสร้างไฟล์ .o ให้โดยอัตโนมัติ
```cmake
add_library(archive STATIC a.cpp b.cpp c.cpp)
```

### Binary Executables
```cmake
#syntax
add_executable(output-name ...)

#example
add_executable(main main.cpp)
```

### Linking
```cmake
#syntax
target_link_libraries(target iterm...)

#example
target_link_libraries(main archive)
```

โค้ดทั้งหมด
```cmake
add_library(objects OBJECT a.cpp b.cpp c.cpp)
add_library(archive STATIC $<TARGET_OBJECTS:objects>)
add_executable(main main.cpp)
target_link_libraries(main archive)
```
หรือ
```cmake
add_library(archive STATIC a.cpp b.cpp c.cpp)
add_executable(main main.cpp)
target_link_libraries(main archive)
```