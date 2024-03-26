## muduo 

reactors in thread +  one loop per thread 

一个线程一个事件循环

<img src="C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240326114056229.png" alt="image-20240326114056229" style="zoom: 50%;" />

workers处理已连接用户的读写事件，当如果有需要做耗时的I/O操作(传送文件和音视频)时，(提交到)需要单独开一个线程进行操作，如果依旧在当前work线程操作，可能会导致当前work线程不能及时处理其他注册的I/O事件

> 一个main reactor 负载accept链接，将链接挂载到多个sub reactor (采用round-robin方法选择sub reactor)，链接注册到哪个线程，所有的读写操作都在那个worker中完成 。
>
> 多个链接可以分派到多个线程，以充分利用CPU
>
> 耗费I/O时间的计算任务，可以提交到创建的ThreadPool中专门处理耗时的任务

## nginx

基于进程设计，采用多个reactor充当I/O进程和工作进程，通过一把accept锁，完美解决多个Reactor的"惊群现象"