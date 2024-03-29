```c++
std::bind(hello,_1)("hello bind");
std::cout << std::bind(sum,10,20) << std::endl;

std::cout << std::bind(&Test::sum,Test(),10,20) << std::endl;
```

bind是函数模板，可以自动推导函数模板参数

占位符_1，表示那里需要填写一个参数

绑定器的弊端，出了语句之后就需要重新写

结合function，可以实现多次使用

```
function<void(string)> func = bind(hello,_1);
func("hello function");
```

### 结合使用，实现线程池

```C++
#include <iostream>
#include <thread>
#include <vector>
#include <functional>

class Thread
{
public:
    Thread(std::function<void()> func) : _func(func) {}
    std::thread Run()
    {
        std::thread t(_func);
        return t;
    }
private:
    std::function<void()> _func;
};

class ThreadPool
{
public:
    ThreadPool() {}
    ~ThreadPool() {
        for(int i = 0;i < workers.size();++i)
        {
            delete workers[i];
        }
    }

    void start(int size)
    {
        for(int i = 0; i < size; ++i)
        {
            workers.push_back(
                new Thread(std::bind(&ThreadPool::RunInThread,this,i))
                );
        }
        for(int i = 0;i < size; ++i)
        {
            handlers.push_back(workers[i]->Run());
        }

        /*
        for(int i = 0;i < size; ++i)
        {
            handlers[i].join();
        }
        */

        for(std::thread& t:handlers)
        {
            t.join();
        }
    }

    
private:
    void RunInThread(int id)
    {
        std::cout << "call inThread id : " << id << std::endl;
    }
    std::vector<Thread*> workers;
    std::vector<std::thread> handlers;    
};

int main()
{
    ThreadPool threadpool;
    threadpool.start(10);

    return 0;
}
```



```C++
#include <iostream>
#include <thread>
#include <vector>
#include <functional>
using namespace std::placeholders;


class Thread
{
public:
    Thread(std::function<void(int)> func , int num) : _func(func),_num(num) {}
    std::thread Run()
    {
        std::thread t(_func,_num); //_func(_num)
        return t;
    }
private:
    std::function<void(int)> _func;
    int _num;
};

class ThreadPool
{
public:
    ThreadPool() {}
    ~ThreadPool() {
        for(int i = 0;i < workers.size();++i)
        {
            delete workers[i];
        }
    }

    void start(int size)
    {
        for(int i = 0; i < size; ++i)
        {
            workers.push_back(
                new Thread(std::bind(&ThreadPool::RunInThread,this,_1),i)
                );
        }
        for(int i = 0;i < size; ++i)
        {
            handlers.push_back(workers[i]->Run());
        }

        /*
        for(int i = 0;i < size; ++i)
        {
            handlers[i].join();
        }
        */

        for(std::thread& t:handlers)
        {
            t.join();
        }
    }

    
private:
    void RunInThread(int id)
    {
        std::cout << "call inThread id : " << id << std::endl;
    }
    std::vector<Thread*> workers;
    std::vector<std::thread> handlers;    
};

int main()
{
    ThreadPool threadpool;
    threadpool.start(10);

    return 0;
}
```

