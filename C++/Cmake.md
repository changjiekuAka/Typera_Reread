## **1.2 编写 CMakeLists.txt**

对于这种情况，CMakeLists.txt 可以有不同的写法：

### **写法 1**

首先看第一种写法，如下：

```cmake
cmake_minimum_required (VERSION 2.8)

project (sum_test)

include_directories (func)

add_executable(sum_test main.c func/sum.c)
```

这里出现了 1 个新的命令：**include_directories**，用来指定[头文件](https://www.zhihu.com/search?q=头文件&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A2359714667})的搜索路径

### **写法 2**

再来看第二种写法，如下：

```cmake
cmake_minimum_required (VERSION 2.8)

project (sum_test)

include_directories (func)

aux_source_directory (func SRC_LIST)

add_executable(sum_test main.c ${SRC_LIST})
```

可以使用 **aux_source_directory**，将指定目录下的**源文件**列表存放到**变量**中



## ADD_SUBDIRECTORY指令

用于添加存放含有源文件的子目录，并可以指定中间文件和目标文件的存放路径

```cmake
ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
```

> EXCLUDE_FROM_ALL 参数的含义是将这个目录从编译过程中排除，比如，工程 的 example，可能就需要工程构建完成后，再进入 example 目录单独进行构建(当然，你 也可以通过定义依赖来解决此类问题)。

```cmake
ADD_SUBDIRACTORY(src bin)
```

假如现在是外部编译，那么中间二进制和目标二进制都存放在 build/bin 目录下，内部编译就会产生一个bin目录在PROJECT_BINARY_DIR下，也就是工程目录下

也可以指定文件的输出路径

```Cmake
SET(EXECUTABLE_OUTPUT_DIR ${PROJECT_BINARY_DIR}/bin)
SET(LIBARY_OUTPUT_DIR ${PROJECT_BINARY_DIR}/lib)
```



这样二进制的输出目录就是在Build/bin 库的输出目录就是Build/lib



## CMAKE_INSTALL_PREFIX变量

定义安装路径

```
cmake -DCMAKE_INSTALL_PREFIX=/usr .
```

INSTALL 指令用于定义安装规则，安装的内容可以包括目标二进制、动态库、静态库以及 文件、目录、脚本等

```Cmake
INSTALL(TARGETS targets...
 	[[ARCHIVE|LIBRARY|RUNTIME]
 		[DESTINATION <dir>]
 		[PERMISSIONS permissions...]
 		[CONFIGURATIONS
	 [Debug|Release|...]]
 		[COMPONENT <component>]
 		[OPTIONAL]
 		] [...])
```

TARGETS 后面跟的就是我们ADD_EXECUTABLE 或者ADD_LIBRARY 的目标文件，可能是可执行二进制、静态库、动态库

- ARCHIVE 表示这是一个静态库
- LIBRARY 表示这是一个动态库
- RUNTIME 表示这是一个可运行的二进制

DESTINATION 表示你要安装的目录是哪里，目录如果以/开头则表明是绝对路径，不是则为相对路径：

${CMAKE_INSTALL_PREFIX} / <DESTINATION> 路径



## CLEAN_DIRECT_OUTPUT



`CLEAN_DIRECT_OUTPUT`是一个 CMake 的目标属性。将该属性设置为 1 时，构建过程不会将输出文件复制到构建树的根目录。而是将输出文件放在指定的位置，以避免与其他同名目标的库发生冲突。



具体来说，当`CLEAN_DIRECT_OUTPUT`属性被设置为 1 时，构建目录中只会生成目标文件，而不会生成复制文件。这样就避免了多个目标文件输出到同一目录下的冲突问题。



CLEAN_DIRECT_OUTPUT 属性可以防止 CMake 删除其他库文件，是因为该属性告诉 CMake 不要删除其它同名的库文件。CMake 在构建新的 target 时会检查目标文件的输出路径是否已经存在同名的文件，如果存在，则会在构建新的 target 前删除该文件。如果设置了 CLEAN_DIRECT_OUTPUT 属性，则会防止 CMake 删除其他同名文件，从而确保同时生成共享库和静态库，并存储在构建目录的 lib 目录中。



## SET_TARGET_PROPERTIES，其基本语法是：

```Cmake
SET_TARGET_PROPERTIES(target1 target2 ...
 PROPERTIES prop1 value1
 prop2 value2 ...)
```

设置输出的名称，对于动态库，还可以用来指定动态库版本和API版本

```cmake
SET_TARGET_PROPERTIES(hello_static PROPERTIES OUTPUT_NAME "hello")
```

动态库版本号 按照规则，动态库是应该包含一个版本号的，我们可以看一下系统的动态库，一般情况是 

libhello.so.1.2 

libhello.so ->libhello.so.1 

libhello.so.1->libhello.so.1.2 

为了实现动态库版本号，我们仍然需要使用 SET_TARGET_PROPERTIES 指令。 具体使用方法如下： 

**SET_TARGET_PROPERTIES(hello PROPERTIES VERSION 1.2 SOVERSION 1)** 

VERSION 指代动态库版本，SOVERSION 指代 API 版本。

