greater a > b

less a < b 

```c++
auto it1 = find_if(vec.begin(),vec.end(),bind1st(greater<>(),70));
```

bind1st把70绑定在greater的第一个参数上，可以找到第一个小于70的迭代器

```C++
#include <iostream>

template<typename Iterator,typename Compare>
Iterator my_bind_if(Iterator A,Iterator B,Compare comp)
{
    auto it = A;
    for(;it != B; ++it){
        if(comp(*A)){
            return it;
        }
    }
}

template<typename Compare, typename T>
class _mybind1st
{
public:
    _mybind1st(Compare comp,T& val) : _comp(comp),_val(val) {}
    bool operator() (const T& second)
    {
        return _comp(val,second);
    }
private:
    Compare _comp;
    T _val;
};


template<typename Compare,typename T>
_mybind1st<Compare,T> mybind1st(Compare comp , T& val){
    return _mybind1st<Compare,T>(comp,T);
}
```

