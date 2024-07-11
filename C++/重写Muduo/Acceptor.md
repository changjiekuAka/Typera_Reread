## Acceptor

该类负责监听新连接的到来，是对listenfd的操作的封装

主要逻辑：创建listenfd ==> 使用listenfd创建一个acceptChannel添加到 ==>  当前线程的loop(mainLoop)的Poller中(enableReading  ==> update  ==> epoll_ctl)  ==>  开始监听listenfd读事件  ==>  读事件发生   ==>   accept新连接   ==>   执行回调newConnectionCallback(connfd，peerAddr)  ==>  由TcpServer定义：根据轮询算法选择一个subloop(getNextLoop函数)，唤醒subloop，把当前connfd封装成Channel分发给subLoop

```C++
void Acceptor::listen()
{
    listenning_ = true;
    acceptSocket_.listen();
    acceptChannnl_.enableReading();
}
```

注意：上述的这些逻辑都发生在主线程当中，主线程在mainLoop中调用newConnectionCallback