```c++
#include <iostream>
#include <string>

template<typename T>
class myfunction;

template <typename R, typename A>
class myfunction<R(A)>
{
public:
    using PFUNC = R(*)(A);
    myfunction(PFUNC pfunc) : _pfunc(pfunc) {}
    R operator() (A arg)
    {
        return _pfunc(arg);
    }
private:
    PFUNC _pfunc;
};

// A为可变参类型
template <typename R, typename... A>
class myfunction<R(A...)>
{
public:
    using PFUNC = R(*)(A...);
    myfunction(PFUNC pfunc) : _pfunc(pfunc) {}
    R operator() (A... arg)
    {
        return _pfunc(arg...);
    }
private:
    PFUNC _pfunc;
};

void hello(std::string str, int num)
{
    std::cout << str << num << std::endl;
}

int main()
{
    myfunction<void(std::string,int)> func(hello);
    func("This is my function",32);
    return 0;
}
```

