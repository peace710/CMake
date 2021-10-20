## CMake解析

### 各种可用变量

CMake语法指定了许多变量，可用于帮助您在项目或源代码树中找到有用的目录

| Variable                 | Info                                                 |
| ------------------------ | ---------------------------------------------------- |
| CMAKE_SOURCE_DIR         | 工程顶层目录，源代码目录，可等同于PROJECT_SOURCE_DIR |
| CMAKE_CURRENT_SOURCE_DIR | 当前处理的CMakeLists.txt所在路径                     |
| PROJECT_SOURCE_DIR       | 工程顶层目录                                         |
| CMAKE_BINARY_DIR         | 运行cmake的目录，外部构建时就是build目录             |
| CMAKE_CURRENT_BINARY_DIR | 当前所在build目录                                    |
| PROJECT_BINARY_DIR       | 等同于CMAKE_BINARY_DIR                               |

在cmakeLists.txt中，可使用message()命令输出这些变量，这些变量不仅可以在cmakeLists.txt中使用，也可以在源代码cpp中使用。

### 源文件变量

创建一个包含源文件的变量，以便于将其轻松的添加到多个命令中，比如add_executable()函数

```cmake
set(SOURCES
    src/Hello.cpp
    src/main.cpp
)

add_executable(${PROJECT_NAME} ${SOURCES})
```

### 包含目录

当您有其他需要包含的文件夹（文件夹里有头文件）时，可以使用以下命令使编译器知道它们：

target_include_directories(),编译此目标时，将使用-l标志将这些目录添加到编译器中，例如-l/dir/path

```cmake
target_include_directories(target
    PRIVATE
        ${PROJECT_SOURCE_DIR}/include
)
```

PRIVATE 标识指定包含的范围。

### 详细输出

使用VERBOSE参数，可达到构建状态的详细输出

```shell
mkdir build
cd build/
cmake ..
make VERBOSE=1
```

