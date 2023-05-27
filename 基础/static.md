# 使用静态变量能查看static调用了多少次。

```cpp

#include <iostream>

bool passOrFail()
{
	static int a = 0;
	if (a <= 3)
	{
		a++;
	}
	return (a > 3? false: true);
	
}

int main()
{
	std::cout << "User #1: " << (passOrFail() ? "Pass\n" : "Fail\n");
	std::cout << "User #2: " << (passOrFail() ? "Pass\n" : "Fail\n");
	std::cout << "User #3: " << (passOrFail() ? "Pass\n" : "Fail\n");
	std::cout << "User #4: " << (passOrFail() ? "Pass\n" : "Fail\n");
	std::cout << "User #5: " << (passOrFail() ? "Pass\n" : "Fail\n");

	return 0;
}
```
