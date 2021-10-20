### 命令作用解析

```cmake
project (hello) #设置工程名
```

CMake构建包含一个项目名称，上面的命令会自动生成一些变量，在使用多个项目时引用某些变量会更加容易。

比如生成了：PROJECT_NAME这个变量。PROJECT_NAME是变量名，${PROJECT_NAME}是变量值 ，值为hello

```cmake
add_executable(hello main.cpp) #生成可执行文件
```

add_executable()命令指定某些源文件生成可执行文件，例子中的是main.cpp，add_executable()函数的第一个参数是可执行文件名，第二个参数是要编译的源文件列表。

### 生成与工程名同名的二进制文件 

```cmake
#cmakeLists.txt
cmake_minimum_required(VERSION 3.17) #设置CMake最小版本
project (hello) #设置工程名
add_executable(${PROJECT_NAME} main.cpp) #生成可执行文件

```

project(hello)函数执行时会生成一个PROJECT_NAME的变量，$(PROJECT_NAME)表示的变量值为hello，添加在add_executable()函数里可以生成执行文件名叫hello

### 外部构建 

使用外部构建，可以创建一个位于文件系统上任何位置的构建文件夹，生成的临时文件和目标文件都位于此目录中，以保持源代码结构目录的整洁。

```shell
mkdir build
cd build
cmake..
```

文件目录结构如下

```
├── build
│   ├── CMakeCache.txt
│   ├── CMakeFiles
│   │   ├── 3.17.3
│   │   │   ├── CMakeCCompiler.cmake
│   │   │   ├── CMakeCXXCompiler.cmake
│   │   │   ├── CMakeDetermineCompilerABI_C.bin
│   │   │   ├── CMakeDetermineCompilerABI_CXX.bin
│   │   │   ├── CMakeRCCompiler.cmake
│   │   │   ├── CMakeSystem.cmake
│   │   │   ├── CompilerIdC
│   │   │   │   ├── CMakeCCompilerId.c
│   │   │   │   ├── a.exe
│   │   │   │   └── tmp
│   │   │   └── CompilerIdCXX
│   │   │       ├── CMakeCXXCompilerId.cpp
│   │   │       ├── a.exe
│   │   │       └── tmp
│   │   ├── CMakeDirectoryInformation.cmake
│   │   ├── CMakeOutput.log
│   │   ├── CMakeTmp
│   │   ├── Makefile.cmake
│   │   ├── Makefile2
│   │   ├── TargetDirectories.txt
│   │   ├── cmake.check_cache
│   │   ├── hello.dir
│   │   │   ├── CXX.includecache
│   │   │   ├── DependInfo.cmake
│   │   │   ├── build.make
│   │   │   ├── cmake_clean.cmake
│   │   │   ├── depend.internal
│   │   │   ├── depend.make
│   │   │   ├── flags.make
│   │   │   ├── link.txt
│   │   │   ├── main.cpp.o
│   │   │   └── progress.make
│   │   └── progress.marks
│   ├── Makefile
│   ├── cmake_install.cmake
│   └── hello.exe
├── cmakeLists.txt
└── main.cpp
```

执行make生成hello.exe可执行文件，执行./hello.exe运行应用