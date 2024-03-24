# 重写Login方法

Protobuf生成了Login的rpc调用方法，我们需要重写服务端的Login函数

```c++
void Login(::google::protobuf::RpcController* controller,
                       const ::fixbug::LoginRequest* request,
                       ::fixbug::LoginResponse* response,
                       ::google::protobuf::Closure* done)
    {

    }
```

参数中的request已经实现了请求参数从字节流  ==》message数据类型的反序列化

```C++
void Login(::google::protobuf::RpcController* controller,
                       const ::fixbug::LoginRequest* request,
                       ::fixbug::LoginResponse* response,
                       ::google::protobuf::Closure* done)
    {
        /*
        注意这里，数据以字节流的形式传输，此时request指针已经是由Protobuf帮我们进行反序列化后的数据类型
        好处：我们无需关心数据的序列化和反序列化，Protobuf已经帮我们做好了
        */
        std::string name = request->name();
        std::string pwd = request->pwd();

        //编写本地业务
        bool login_result = Login(name,pwd);

        //编写响应 
        fixbug::ResultCode* result = response->mutable_result();
        result->set_errcode(0);
        result->set_errmsg("");
        response->set_success(login_result);
        
        //执行回调
        done->Run();
    }
```

这里只是编写业务代码，这里的Login方法，也是由RPC框架来调用的，我们需要去实现这个框架

这里的Run()函数是一个纯虚函数，也就代表这里的closure是一个抽象类。

意味着需要去继承这个抽象类，重写其中的Run函数，这里的Run函数其中理所当然的应该去实现响应消息的序列化和发送

```
class PROTOBUF_EXPORT Closure {
 public:
  Closure() {}
  virtual ~Closure();

  virtual void Run() = 0;

 private:
  GOOGLE_DISALLOW_EVIL_CONSTRUCTORS(Closure);
};
```

