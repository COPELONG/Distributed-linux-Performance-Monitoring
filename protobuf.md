# Protobuf



![image-20231230212544342](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231230212544342.png)

序列化：内存到磁盘。

反序列化：磁盘到内存。

打包成大块数据块然后转化成字符串形式进行传输。

![image-20231230221428132](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231230221428132.png)

```c++
// Person.proto
syntax = "proto3";

// 在该文件中对要序列化的结构体进行描述
message Person
{
    int32 id = 1;
    bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
}

```

![image-20231230221843143](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231230221843143.png)

## 数组类型：

```c++
// 要序列化的数据
struct Person
{
    int id;
    string name[10];
    string sex;	
    int age;
};
message Person
{
    int32 id = 1;
    repeated bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
}
```

![image-20231231103900701](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231231103900701.png)

add_name()先申请一块内存，在使用set_name赋值。



## 枚举类型：

```c++
// 要序列化的数据
// 枚举
enum Color
{
    Red = 5,	// 可以不给初始值, 默认为0
    Green,
    Yellow,
    Blue
};
----------------------------------------------------------------------------
// 定义枚举类型
enum Color
{
    Red = 0;
    Green = 3;		// 第一个元素以外的元素值可以随意指定
    Yellow = 6;
    Blue = 9;
}
// 在该文件中对要序列化的结构体进行描述
message Person
{
    int32 id = 1;
    repeated bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
    // 枚举类型
    Color color = 5;
}
```



## 导入其它protobuf文件：

![image-20231231104329433](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231231104329433.png)

两个protobuf都需要使用命令行生成cc和h。

![image-20231231104509784](https://my-figures.oss-cn-beijing.aliyuncs.com/Figures/image-20231231104509784.png)



## 包：

在proto文件中定义，相当于c++中的命名空间

```c++
syntax = "proto3";
// 添加命名空间 Dabing
package Dabing;

// 地址信息, 这个Address类属于命名空间: Dabing
message Address
{
    bytes addr = 1;
    bytes number = 2;
}
-------------------------------------------------------------------------------------------------

syntax = "proto3";
// 使用另外一个proto文件中的数类型, 需要导入这个文件
import "Address.proto";
// 指定命名空间 ErBing
package ErBing;

// 以下的类 Person 和枚举 Color 都属于命名空间 ErBing
// 在该文件中对要序列化的结构体进行描述
// 定义枚举类型
enum Color
{
    Red = 0;
    Green = 3;		// 第一个元素以外的元素值可以随意指定
    Yellow = 6;
    Blue = 9;
}
// 在该文件中对要序列化的结构体进行描述
message Person
{
    int32 id = 1;
    repeated bytes name = 2;
    bytes sex = 3;	
    int32 age = 4;
    // 枚举类型
    Color color = 5;
    // 添加地址信息, 使用的是外部proto文件中定义的数据类型
    // 如果这个外边类型属于某个命名空间, 语法格式:
    // 命名空间的名字.类名 变量名=编号;
    Dabing.Address addr = 6;
}
```



--------------------

------------------------------------



## CMakeList中的protobuf语法：

```cmake
find_package(protobuf CONFIG REQUIRED)
find_package(gRPC CONFIG REQUIRED)
find_package(c-ares CONFIG)
# find_package(Threads)

#
# Protobuf/Grpc source files
#
set(PROTO_FILES
    monitor_info.proto
    cpu_load.proto
    cpu_softirq.proto
    cpu_stat.proto
    mem_info.proto
    net_info.proto
set(PROTO_FILES
    monitor_info.proto
    cpu_load.proto
    cpu_softirq.proto
    cpu_stat.proto
    mem_info.proto
    net_info.proto
)

#
# Add Library target with protobuf sources
#
add_library(monitor_proto ${PROTO_FILES})
target_link_libraries(monitor_proto
    PUBLIC
        protobuf::libprotobuf
        gRPC::grpc
        gRPC::grpc++
)
target_include_directories(monitor_proto PUBLIC
${PROTOBUF_INCLUDE_DIRS} 
${CMAKE_CURRENT_BINARY_DIR})#表示当前的构建目录。主要用于包含由 Protocol Buffers 生成的头文件，通常是 .pb.h 文件。

#
# Compile protobuf and grpc files in mointor proto target to cpp
#
get_target_property(grpc_cpp_plugin_location gRPC::grpc_cpp_plugin LOCATION)
protobuf_generate(TARGET monitor_proto LANGUAGE cpp) #等价于 protoc 命令
protobuf_generate(TARGET monitor_proto LANGUAGE grpc GENERATE_EXTENSIONS .grpc.pb.h .grpc.pb.cc PLUGIN "protoc-gen-grpc=${grpc_cpp_plugin_location}")
```











