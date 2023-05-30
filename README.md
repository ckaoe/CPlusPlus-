# 学习c++过程中的笔记。

## 重点
**左值：** 可以长时间保存，可以存在于=左边的值，可以取地址；
**右值：** 临时值，不能存在于=左边的值，不可以取地址。

----
## 关键词
|关键词|描述|示例|
|----|----|----|
|`static`|让元素只具备链接，只能在定义`static`的元素中使用|1.0|
|`extern`|变量的外部链接|1.0|
|`inline`|定义内联命名空间|1.1|
|`static_cast<>()`|类型转换|1.2|
|`->`|从指针操作符或箭头操作符，可以用来从指向对象的指针中选择成员|1.3|


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

## 箭头操作符 1.3
```cpp
#include <iostream>
using namespace std;

struct Employee
{
    int id{};
    int age{};
    double wage{};

};

int main()
{
    Employee joe{1,2,3};
    Employee* jo = &joe;

    // 从指针操作符（->）,可以用来从指向对象的指针中选择成员。
    cout << jo->age << endl;

    return 0;
}
```
