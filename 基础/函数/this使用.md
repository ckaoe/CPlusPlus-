# this的使用
## 明确引用“this”

大多数情况下，您永远不需要显式引用“this”指针。但是，在某些情况下，这样做可能会很有用：

首先，如果你有一个构造函数（或成员函数），它的一个参数和一个成员变量同名，你可以使用“this”来消除它们的歧义：

```c++
#include<iostream>

class Something
{
private:
	int data;

public:
	Something(int data)
	{
		// 和Java中的this有类似的功能。
		this->data = data;
	}

	void getInfo()
	{
		std::cout << "数字：" << data << std::endl;
	}
};


int main()
{

	Something some(88);
	some.getInfo();

	
	return 0;
}
```

请注意，我们的构造函数接受一个与成员变量同名的参数。在本例中，“data”指的是参数，“this->data”指的是成员变量。虽然这是可以接受的编码实践，但我们发现在所有成员变量名上使用“m_”前缀提供了更好的解决方案，可以完全防止重复的名称！

一些开发人员更喜欢显式地将this-添加>到所有类成员中。我们建议您避免这样做，因为它往往会使您的代码可读性较差，而且没有什么好处。使用m_前缀是区分成员变量和非成员（局部）变量的更易读的方法。

## 请尽量使用这种用法。
```c++
class Something
{
private:
	int m_data;

public:
	Something(int data): m_data{data} {}
};

```

# （可链接）通过*this来简写函数
```c++
#include<iostream>

class Calc
{
private:
	int value;
public:
	// 返回value
	Calc& add(int m_value) { value += m_value; return *this; }
	Calc& adds(int m_value) { value += m_value; return *this; }
};


int main()
{
	int* b = nullptr;
	int& a{*b};
	
	return 0;
}
```
