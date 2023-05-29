# 学习c++过程中的笔记。
## 关键词
|关键词|描述|示例|
|----|----|----|
|`static`|让元素只具备链接，只能在定义`static`的元素中使用|1.0|
|`extern`|变量的外部链接|1.0|
|`inline`|定义内联命名空间|1.1|
|`static_cast<>()`|类型转换|1.2|



## 示例1.0
文件a.cpp
```cpp
#include <iostream>
using namespace std;
    
static void a()
{
    cout<<"hello world"<<endl;
}

void h()
{
    cout<<"hello world"<<endl;
}

extern const int g_y{ 3 };  // 默认情况下，非const全局变量是外部变量（如果使用，则会忽略extern关键字）。
int g_x{ 2 }; // 如果没有const则本身就具有外部链接。
```

文件main.cpp
```cpp
#include <iostream>
#include "a.h"
using namespace std;

extern const int g_y;
extern int g_x;
    
int main()
{
    a(); // 就算再头文件中声明了函数a(), 在其他文件中也访问不到这个函数。
    h(); // 没有static声明的函数，可以被访问到。
    return 0;
}
```

## 定义内联命名空间 1.1
```cpp
inline namespace v1
{
    void doSomething()
    {
        std::cout << "v1\n";
    }
}

int main()
{
    // 内联命名空间的访问。
    doSomething();

    return 0;
}
```

## 类型转换 1.2
```cpp
static_cast<Pet>(2)
```
