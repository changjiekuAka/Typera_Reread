![image-20240421143736149](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240421143736149.png)

![image-20240421143802798](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240421143802798.png)

> ​	其中阻塞、非阻塞和IO复用，应用程序除了**阻塞住等待数据就绪去读取**，或者就是不断地去**轮询的检查fd中是否有数据**，最终依旧**由应用程序去读取和写入数据**，所以这三种IO模型进行IO的方式，消耗的都是应用程序的时间，应用程序在调用了一个IO接口后，就不能再去做其他事情，只有阻塞或者轮询检查数据就绪状态。**结论：这三种IO模型都是同步IO**

![image-20240421143826340](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240421143826340.png)