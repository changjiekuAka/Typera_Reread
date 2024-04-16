## Provider和Consumer的通信报文格式

现在已经可以像Provider中注册服务，并且记录服务的类型以及服务中所拥有的方法，也是根据这个建立了一张表描述**服务名称和服务描述指针** **方法名称和方法描述指针** 他们的对应关系表

```c++
 // 组织服务对象和服务中所有的方法对象
    struct ServiceInfo
    {
        google::protobuf::Service *m_service; // 服务对象
        std::unordered_map<std::string,const google::protobuf::MethodDescriptor*> m_methodMap;
    };
    // 一个provider节点中有多个服务，组织各个服务名称和服务
    std::unordered_map<std::string,ServiceInfo> m_serviceMap;
```

有了这个表之后，使用这个表需要两个变量一个是 service_name 还有 method_name ，当Provider知道了这两个参数后就可以提取服务描述指针和方法描述指针，**对于这两个变量，在proto文件中编写message类型进行序列化和反序列化**

报文中所应有的数据

- **service_name  **
- **method_name  **
- **args**

其中**service_name**和**method_name**做数据头**args**为请求方法的参数

为了区别数据头和请求方法参数，需要在数据头前面加上数据头的长度(4字节) **header_size**

> 注意这里header_size读取时为4字节，意味着存储这个数据不能直接以字符串的形式存入，原因：如果数字为10000，字符串形式存入就需要读取五个字节

此处在proto文件中，针对数据头做**message**封装

> 这里不需要对**args**进行**message**封装，因为**args**参数是需要用户在使用这个框架的时候，注册服务方法时，自己去定义参数的类型和个数，对于框架来讲不需要关心这些事情，所以也只是针对数据头进行**message**封装

除了区分**数据头**和**args**要使用**header_size**，**TCP传输**中以**字节流**的形式传输，所以会存在**TCP粘包**的问题，这个问题关键在于：**无法区分**第一个数据包中args与第二个数据包中的数据头，所以针对这个问题，还需要对数据头中加入一个数据**args_size**

```protobuf
message
{
	bytes service_name;
	bytes method_name;
	args_size;
}
```

