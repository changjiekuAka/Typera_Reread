## Logger日志系统

单独创建一个线程负责从Lock_queue中取出日志信息，存入文件中

### logger

```C++
#include "logger.h"

Logger &Logger::GetInstance()
{
    static Logger ins;
    return ins;
}

Logger::Logger()
{
    std::thread WriteLogTask([&]()
                             {
        while(true){
        time_t now = time(nullptr);
        tm *nowtm = localtime(&now);
		// 使用相对路径，创建不同日期的日志文件
        char log_txt[256] = {0};
        sprintf(log_txt,"./log/%d-%d-%d.txt",nowtm->tm_year + 1900,nowtm->tm_mon + 1,nowtm->tm_mday);

        FILE *fp = fopen(log_txt,"a+");
        if(fp == nullptr){
            std::cout << " Log file: " << log_txt << "open error !" << std::endl;
            exit(EXIT_FAILURE);
        }
        std::string msg = m_lockqueue.pop();

        char buffer[128] = {0};
        sprintf(buffer,"[%d-%d-%d][%s]",
                        nowtm->tm_hour,
                        nowtm->tm_min,
                        nowtm->tm_sec,
                        (m_loglevel == INFO ? "INFO" : "ERROR"));
        msg.insert(0,buffer);
        msg.append("\n");

        fputs(msg.c_str(),fp);
        fclose(fp);
        } });
    WriteLogTask.detach();
}

void Logger::Log(const std::string &msg)
{
    m_lockqueue.push(msg);
}

void Logger::SetLogLevel(LogLevel level)
{
    m_loglevel = level;
}
```

### lockqueue

```C++
#pragma once
#include <thread>
#include <condition_variable>
#include <mutex>
#include <queue>

// 异步写日志的异步队列
template<typename T>
class LockQueue
{
public:
    void push(const T& data){
        std::lock_guard<std::mutex> lock(m_mutex);
        m_queue.push(data);
        m_condvariable.notify_one();
    }

    T pop(){
        std::unique_lock<std::mutex> lock(m_mutex);
        while(m_queue.empty())
        {
            m_condvariable.wait(lock);
        }
        T data = m_queue.front();
        m_queue.pop();
        return data;
    }
private:
    std::queue<T> m_queue;
    std::mutex m_mutex;
    std::condition_variable m_condvariable;
};

```

