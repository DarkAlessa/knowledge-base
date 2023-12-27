### -D Option
-D เป็น option ที่ใช้ควบคุม preprocessor ของคอมไพล์เลอร์
```
-D name
-D name=definition
```

#### main.cpp
```c++
#include <iostream>

int main() {
	#ifdef DEBUG 
		std::cout << "Debug version" << '\n';
	#else
		std::cout << "Release version" << '\n';
	#endif

    #ifdef NUMBER
		#if NUMBER == 10
			std::cout << "NUMBER 10" << '\n';
		#elif NUMBER == 20
			std::cout << "NUMBER 20" << '\n';
		#endif
	#endif

}
```

#### g++ compiler (-D) 
```bash
g++ -DDEBUG -DNUMBER=20 main.cpp -o main
```

#### CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.15)
project(demo)

add_executable(main main.cpp)
target_compile_definitions(main PRIVATE DEBUG NUMBER=20)
```
output
```
Debug mode
NUMBER 20
```
