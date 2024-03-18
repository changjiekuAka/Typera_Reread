# Protobuf如何实现RPC方法的序列化和反序列化

对于远端进程需要知道想要调用的方法是什么，就需要把要调用的函数、参数，进行序列化，发送给远端，这样远端才能知道要调用的函数是什么

Protobuf不提供RPC的功能

只是提供对于RPC函数的序列化和反序列化

### 如何在Protobuf里面描述rpc方法

```protobuf
service UserServiceRpc
{
    rpc Login(LoginRequest) returns(LoginResponse);
}
```

定义下面的选项，表示生成service服务类和rpc方法描述

```protobuf
option cc_generic_services = true;
```

