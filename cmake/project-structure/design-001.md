```
📁 Project
 ├─📁 src
 ├─📁 lib
 │  ├─📄 CMakeLists.txt    ---(2)
 │  ├─📁 lib-a
 │  │  ├─📁 src
 │  │  └─📁 include
 │  └─📁 lib-b
 │     ├─📁 src
 │     └─📁 include
 ├─📁 include
 ├─📁 bin
 ├─📁 build
 ├─📁 test
 └─📄 CMakeLists.txt       ---(1)
```

#### Project/CMakeLists.txt ---(1)
```cmake
cmake_minimum_required(VERSION 3.20)
project(dirtest C CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wpedantic")

# set .exe file output dir
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/bin")
# set achive/static (.lib, .a) flie output dir
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib")
# set shared/dynamic (.dll, .so) file output dir
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib")

add_subdirectory(lib)

file(GLOB src ${CMAKE_SOURCE_DIR}/src/*.cpp)

list(REMOVE_ITEM src ${CMAKE_SOURCE_DIR}/src/main.cpp)

list(LENGTH src LIST_LENGTH)
if(${LIST_LENGTH})
  message("List is not empty")
  add_library(objs OBJECT ${src})
  target_include_directories(obj PRIVATE
    ${CMAKE_SOURCE_DIR}/include
  )
endif()

add_executable(main ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp)
target_include_directories(main PRIVATE
  ${CMAKE_SOURCE_DIR}/include
  ${CMAKE_SOURCE_DIR}/lib/lib-a/include
  ${CMAKE_SOURCE_DIR}/lib/lib-b/include
)

target_link_libraries(main 
  $<$<TARGET_EXISTS:obj>:obj>
  lib-a
  lib-b
)

#[[
⚠ ถ้าใช้ไฟล์ลิสจะเกิด error ตอน --build เพราะ .a ไฟล์ยังไม่ถูกสร้างจึงไม่สามารถ link ได้
จะใช้ได้ก็ต่อเมื่อ .a ไฟล์ถูกสร้างแล้วเท่านั้น (เช่นการนำเข้า lib จากข้างนอกโปรเจค)
    file(GLOB libs ${PROJECT_SOURCE_DIR}/lib/*.a)
    target_link_libraries(main ${libs})
]]

```

#### Project/lib/CMakeLists.txt ---(2)
```cmake
# lib-a library

file(GLOB lib-a_src ${CMAKE_CURRENT_SOURCE_DIR}/lib-a/src/*.cpp)

add_library(lib-a STATIC ${lib-a_src})
target_include_directories(lib-a PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/lib-a/include
)

# lib-b library

file(GLOB lib-b_src ${CMAKE_CURRENT_SOURCE_DIR}/lib-b/src/*.cpp)

add_library(lib-b STATIC ${lib-b_src})
target_include_directories(lib-b PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/lib-b/include
)
```