## ✗  IO阻塞和非阻塞

------

网络IO分为两个阶段：数据准备和数据读写

在数据准备阶段有 **阻塞** 和 **非阻塞**

```c++
#include <sys/types.h>
#include <sys/socket.h>

ssize_t size = recv(int sockfd, void *buf, size_t len, int flags);
```

size == -1  && errno == EAGAIN 或者是 errno == EWOULDBLOCK ：表示读取内部出错了，或者，表示暂时没有网络数据到来，非阻塞返回`连接是正常的`

size == 0 ：对端连接断开

size > 0：数据正常到来

在数据准备阶段，sockfd中有数据到来，这些数据会被暂存到操作系统的TCP缓冲区中，为数据读写阶段做准备

------

### ✗  阻塞和非阻塞对调用线程的影响

阻塞IO，调用IO方法的线程进入阻塞

非阻塞IO，不会改变调用线程的状态，通过返回值判断

------

### ✗  数据读写的同步和异步

同步就像是上述所说的recv函数，由应用程序自己去读取TCP系统缓冲区，再写到应用程序提供的缓冲区buf中，这一切都是由应用程序自己做的，所有的IO性能消耗也累加在应用程序上

异步不同就在于，应用程序调用了异步IO接口，会把所有IO操作所需要的参数(sockfd、buf、buflen)都传给操作系统，其中由于是异步操作，就必须有一个通知操作，所以还需要传给操作系统一个sigio信号，也可以是一个回调函数，当通知操作发生的时候就说明，buf中的已经准备好数据了

muduo库的作者陈硕说了一句话：在处理IO的时候，阻塞和非阻塞都是同步IO，只有使用了特殊的API才是异步IO

![image-20240420122917634](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240420122917634.png)