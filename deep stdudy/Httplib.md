# Httplib

*请求结构体和响应结构体：Request；Response*

### vector数组<请求正则表达式，处理函数>

| 请求信息    | 处理函数      |
| ----------- | ------------- |
| POST/&Login | void Login()  |
| GET/&Serach | void Serach() |

### 线程池

以下是线程处理请求的相关步骤

- 接受请求数据
- 解析请求数据，生成Request结构体
- 检查映射表，根据请求的方法和资源路径查看是否有匹配的处理函数，有则处理
- 将Request结构体和空的Response结构体传入处理函数，返回填好信息的Response结构体
- 根据其中数据组织http响应发回客户端
- 短连接则立刻关闭，长连接则等待超时后关闭

### 其他接口

<img src="C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20230713221911118.png" alt="image-20230713221911118" style="zoom:67%;" />

用于向上面表格中添加数据

```C++
void Login(Request& req,Response& rsp){......}
POST("/login",Login);
```

当收到`POST`请求方法和`login`请求路径时，调用`Login`函数。