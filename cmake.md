All problems in computer science can be solved by another level of indirection.
Make工具有很多种，不同的Make工具遵循着不同的规范和标准，因此其执行的Makefile文件也千差万别。对与跨平台编译，如果用上面的Make工具就需编写不同标准的Makefile文件，这样就很麻烦。
Cmake就是针对这一问题，通过编写CMakeLists.txt文件来制定编译流程，生成Makefile文件。




### cmake步骤
```
1.编写CMakeLists.txt文件
2.cmake
3.make
```

### 常用语法：
```CMake
cmake_minimum_required(VERSION 3.10) 指定版本

project(armor) 工程名称

set(CMAKE_BUILD_TYPE RELEASE) 指定编译类型

include_directories(include) 添加头文件目录

file(GLOB SOURCE "src/*.cpp" "main.cpp") 源文件

add_library(armor SHARED ${SOURCE}) 生成动态链接库

add_library(armor STATIC ${SOURCE}) 生成静态链接库

set(PROJECT_LINK_LIBS libarmor.so) 将库文件存到变量中

link_directories(/home/wenda/cmake/demo_4/build) 添加外部库的搜索路径

target_link_libraries(main ${PROJECT_LINK_LIBS}) 链接外部库文件

add_executable(main main.cpp) 将源文件生成可执行文件
```

### Demo

demo_1 <br>
├── CMakeLists.txt<br>
└── main.cpp<br>
<br>
``` CMake
cmake_minimum_required(VERSION 3.10) #指定版本号

project(main) #工程名称

add_executable(main main.cpp) #把源文件生成可执行文件
```

<br>

demo_2<br>
├── CMakeLists.txt<br>
├── main.cpp<br>
└── print_.cpp<br>
<br>
``` CMake
cmake_minimum_required(VERSION 3.10) #指定版本

project(main) #工程名称

aux_source_directory(. dir_srcs) #将目录下的所有源文件存到变量中

add_executable(main ${dir_srcs}) #把源文件生成可执行文件
```

<br>

demo_3 <br>
├── build<br>
├── CMakeLists.txt<br>
├── include<br>
│&emsp;├── input_A.h<br>
│&emsp;└── input_B.h<br>
├── main.cpp<br>
└── src<br>
 &emsp; ├── input_A.cpp<br>
 &emsp; └── input_B.cpp<br>
 
<br>

``` CMake
cmake_minimum_required(VERSION 3.10) #指定版本

project(main) #工程名称

include_directories(include) #添加头文件目录

#set(SOURCE main.cpp input_A.cpp input_B.cpp) #源文件

file(GLOB SOURCE "src/*.cpp" "main.cpp") #源文件

add_executable(main ${SOURCE}) #把源文件生成可执行文件
```

<br>

demo_4 生成动态库文件<br>
├── build<br>
├── CMakeLists.txt<br>
├── include<br>
│ &emsp;└── armor.h<br>
└── src<br>
  &emsp;└── armor.cpp<br>
<br>

``` CMake
cmake_minimum_required(VERSION 3.10) #指定版本

set(CMAKE_BUILD_TYPE RELEASE) #指定编译类型

project(armor) #工程名称

include_directories(include) #添加头文件目录

file(GLOB SOURCE "src/*.cpp") #源文件

add_library(armor SHARED ${SOURCE}) #生成动态链接库
```

<br>

demo_5 生成静态库文件<br>
├── build<br>
├── CMakeLists.txt<br>
├── include<br>
│&emsp;   └── armor.h<br>
└── src<br>
&emsp; └── armor.cpp<br>
<br>
```CMake
cmake_minimum_required(VERSION 3.10) #指定版本

set(CMAKE_BUILD_TYPE RELEASE) #指定编译类型

project(armor) #工程名称

include_directories(include) #添加头文件目录

file(GLOB SOURCE "src/*.cpp") #源文件

add_library(armor STATIC ${SOURCE}) #生成静态链接库
```

<br>

demo_6 调用动态库或者静态库<br>
├── build<br>
├── CMakeLists.txt<br>
└── main.cpp<br>
<br>
``` CMake
cmake_minimum_required(VERSION 3.10) #指定版本

project(main) #工程名称

set(PROJECT_LINK_LIBS libarmor.so) #将库文件存到变量中

#set(PROJECT_LINK_LIBS libarmor.a)

link_directories(/home/wenda/cmake/demo_4/build) #添加外部库的搜索
路径

#link_directories(/home/wenda/cmake/demo_5/build)

include_directories(../demo_4/include) #包含库对应的头文件

#include_directories(../demo_5/include)

add_executable(main main.cpp)

target_link_libraries(main ${PROJECT_LINK_LIBS}) #链接外部库文件
``` 


### 输出文件的目录
``` CMake
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/output) 可执行文件目录

set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib) 库文件目录
```
### cmake官方链接：
```
https://cmake.org/cmake/help/latest/guide/tutorial/index.html
```
