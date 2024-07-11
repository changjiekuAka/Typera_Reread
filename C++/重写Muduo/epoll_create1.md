![image-20240518205858693](C:\Users\ZZZXXXJJ\AppData\Roaming\Typora\typora-user-images\image-20240518205858693.png)

这个函数的作用和epoll_create1的作用没什么差别，设置了那个标志位后，如果创建epoll实例的进程创建了子进程，并且替换子进程后，这个epoll实例会被关闭

注意：默认情况下，进程替换，子进程会获得父进程的所有fd资源