 ```
 📁 Project
 ├─📁 source
 │  ├─📄 CMakeLists.txt    ---(3)
 │  ├─📁 src
 │  └─📁 include
 ├─📁 lib
 │  ├─📄 CMakeLists.txt    ---(2)
 │  └─📁 lib-a
 │     ├─📁 src
 │     └─📁 include
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
add_subdirectory(source)
```

#### Project/lib/CMakeLists.txt ---(2)
```cmake
# math library

file(GLOB math_src ${CMAKE_CURRENT_SOURCE_DIR}/math/src/*.cpp)

add_library(math STATIC ${math_src})
target_include_directories(math PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/math/include
)

# text library

file(GLOB text_src ${CMAKE_CURRENT_SOURCE_DIR}/text/src/*.cpp)

add_library(text STATIC ${text_src})
target_include_directories(text PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/text/include
)
```

#### Project/source/CMakeLists.txt ---(3)
```cmake
file(GLOB src ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)

list(REMOVE_ITEM src ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp)

list(LENGTH src LIST_LENGTH)
if(${LIST_LENGTH})
  message("List is not empty")
  add_library(objs OBJECT ${src})
  target_include_directories(obj PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
  )
endif()

add_executable(main ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp)
target_include_directories(main PRIVATE
  ${CMAKE_SOURCE_DIR}/source/include
  ${CMAKE_SOURCE_DIR}/lib/math/include
  ${CMAKE_SOURCE_DIR}/lib/text/include
)

target_link_libraries(main 
  $<$<TARGET_EXISTS:obj>:obj>
  math
  text
)
```