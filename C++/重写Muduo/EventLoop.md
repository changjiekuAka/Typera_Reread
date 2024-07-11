## EventLoop

<img src="C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240509221440563.png" alt="image-20240509221440563" style="zoom: 67%;" />

EventLoop就像这里的事件分发器，其中包含了Poller和Channel，Poller监听Channel中fd关心的事件，事件发生调用Channel绑定的回调函数，一个EventLoop可以有很多的Channel，但是Channel只能属于一个EventLoop

> 注意：这里的事件发生时调用Channel绑定的回调函数是这样进行的，Poller在监听事件，调用Epoll_wait的时候出入进去的是一个数组，这个数组用来存储哪些事件就绪了，最终这个数组元素的类型也就是epoll_event，其中epoll_event 有两个变量，一个是关心的事件是什么，第二个变量是一个union类型，其中包含许多的属性，其中有监听的fd和指向Channel的ptr

铭记：One loop per thread

一个线程一个EventLoop

<img src="C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240523223051888.png" alt="image-20240523223051888" style="zoom:67%;" />