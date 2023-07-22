# 智能指针

### auto_ptr

auto_ptr，赋值实际相当于资源管理权的交换，所以资源的原来的指针会被置空

### unique_ptr

unique_ptr，拒绝赋值

### shared_ptr

shared_ptr，可以赋值，采用引用计数的方式，查看资源被那些指针关联

### weak_ptr

weak_ptr，解决循环引用的问题

[(167条消息) 【c++复习笔记】——智能指针详细解析(智能指针的使用,原理分析)_c++ 智能指针_努力学习的少年的博客-CSDN博客](https://blog.csdn.net/sjp11/article/details/123899141)

