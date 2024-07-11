std::atomic_int Thread::numCreated_ = 0;

初始化报错问题

std::atomic_int Thread::numCreated_ = {0};

最后直接调用初始化列表解决，这里判定是拷贝构造赋值，原子变量不允许初始化时这样赋值

[c++ - "Use of deleted function"std::atomic_int 错误 - IT工具网 (coder.work)](https://www.coder.work/article/34740)