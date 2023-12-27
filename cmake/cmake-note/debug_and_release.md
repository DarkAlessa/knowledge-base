### Single-configuration generator (Ninja/Unix-Makefiles)
```
📁 Alice
 ├─📁 bin
 │  ├─📄 alice.exe
 │  └─📄 alice_d.exe
 ├─📁 build
 │  ├─📁 debug
 │  └─📁 release
 ├─📄 alice.cpp
 └─📄 CMakeLists.txt
 ```

📄 CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.0)
project(alice)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

message("Mode :" ${CMAKE_BUILD_TYPE})

add_executable(${PROJECT_NAME} alice.cpp)

set_target_properties(${PROJECT_NAME} 
    PROPERTIES
        DEBUG_POSTFIX "_d"
        CXX_STANDARD 20
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
)

target_compile_options(${PROJECT_NAME} PRIVATE
    $<$<CONFIG:Debug>:-g;-O0>
    $<$<CONFIG:Release>:-O2>
    ${CMAKE_CXX_FLAGS}
    -Wall
    -Wpedantic
)
```

```bash
cmake -S . -B ./build/debug -DCMAKE_BUILD_TYPE=Debug -G "MSYS Makefiles"
cmake -S . -B ./build/release -DCMAKE_BUILD_TYPE=Release -G "MSYS Makefiles"

cmake --build ./build/debug
cmake --build ./build/release
```
---

### Multi-configuration generators (Ninja Multi-Config, Visual Studio)
```bash
cmake -S . -B ./build -DCMAKE_CONFIGURATION_TYPES=Debug;Release -G "MSYS Makefiles"

cmake --build ./build --config Debug
cmake --build ./build --config Release
```