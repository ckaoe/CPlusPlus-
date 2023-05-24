# 学习c++过程中的笔记。
## 关键词
|关键词|描述|示例|
|----|----|----|
|`static`|让元素只具备链接，只能在定义`static`的元素中使用|1.0|



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
```

文件main.cpp
```cpp
#include <iostream>
#include "a.h"
using namespace std;
    
int main()
{
    a(); // 就算再头文件中声明了函数a(), 在其他文件中也访问不到这个函数。
    h(); // 没有static声明的函数，可以被访问到。
    return 0;
}
```
