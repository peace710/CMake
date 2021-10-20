### 创建静态库

add_library()函数用于某些源文件创建一个库，默认生成在构建文件夹

```cmake
add_library(hello_library STATIC src/Hello.cpp)
```

在add_library调用中包含了源文件，用于创建名称为libhello_library.a的静态库

### 添加头文件所在的目录

使用target_include_directories()添加一个目录，这个目录是库所包含的头文件目录，并设置库属性为PUBLIC

```cmake
target_include_directories(hello_library
    PUBLIC
        ${PROJECT_SOURCE_DIR}/include
)
```

使用这个函数后，该目录会在以下情况被调用

编译这个库的时候，因为这个库由hello_library由Hello.cpp生成，Hello.cpp中函数的定义在Hello.h中，Hello.h在这个include目录下，所以显然编译该库的时候，这个目录会被调用到

编译链接到这个hello_library的任何其他目标（库或者可执行文件）



```bash
cmake-test/                 工程主目录，main.c 调用 libhello-world.so
├── CMakeLists.txt
├── hello-world             生成 libhello-world.so，调用 libhello.so 和 libworld.so
│   ├── CMakeLists.txt
│   ├── hello               生成 libhello.so 
│   │   ├── CMakeLists.txt
│   │   ├── hello.c
│   │   └── hello.h         libhello.so 对外的头文件
│   ├── hello_world.c
│   ├── hello_world.h       libhello-world.so 对外的头文件
│   └── world               生成 libworld.so
│       ├── CMakeLists.txt
│       ├── world.c
│       └── world.h         libworld.so 对外的头文件
└── main.c
```

**调用关系：**

```bash
                                 ├────libhello.so
可执行文件────libhello-world.so
                                 ├────libworld.so
```

### 关键字用法说明：

**PRIVATE**：私有的。生成 libhello-world.so时，只在 hello_world.c 中包含了 hello.h，libhello-world.so **对外**的头文件——hello_world.h 中不包含 hello.h。而且 main.c 不会调用 hello.c 中的函数，或者说 main.c 不知道 hello.c 的存在，那么在 hello-world/CMakeLists.txt 中应该写入：

```cmake
target_link_libraries(hello-world PRIVATE hello)
target_include_directories(hello-world PRIVATE hello)
```

**INTERFACE**：接口。生成 libhello-world.so 时，只在libhello-world.so **对外**的头文件——hello_world.h 中包含 了 hello.h， hello_world.c 中不包含 hello.h，即 libhello-world.so 不使用 libhello.so 提供的功能，只使用 hello.h 中的某些信息，比如结构体。但是 main.c 需要使用 libhello.so 中的功能。那么在 hello-world/CMakeLists.txt 中应该写入：

```cmake
target_link_libraries(hello-world INTERFACE hello)
target_include_directories(hello-world INTERFACE hello)
```

**PUBLIC**：公开的。**PUBLIC = PRIVATE + INTERFACE**。生成 libhello-world.so 时，在 hello_world.c 和 hello_world.h 中都包含了 hello.h。并且 main.c 中也需要使用 libhello.so 提供的功能。那么在 hello-world/CMakeLists.txt 中应该写入：

```cmake
target_link_libraries(hello-world PUBLIC hello)
target_include_directories(hello-world PUBLIC hello)
```

实际上，这三个关键字指定的是目标文件依赖项的使用**范围（scope）**或者一种**传递（propagate）**

