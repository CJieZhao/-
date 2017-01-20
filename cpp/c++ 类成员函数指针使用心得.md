#c++ 类成员函数指针使用心得

```cpp
    class IBase
	{
	public:
    	virtual ~IBase() {}
    	virtual int Run(const char *appName, int idx = 0);

    	typedef void* (IBase::*function)();
    	std::vector<function> _funcVec;
	};


	class CTest : public IBase
	{
	public:
    	CTest()
		{
			_funcVec.push_back(reinterpret_cast<function>(&CTest::Test1));
		}

    	virtual ~CTest();
    	virtual int Run(const char *appName, int idx = 0)
		{
			void *p = (this->*_funcVec[idx])();
		}
		
		void* Test1();
	};

```
1. 基类定义, 基类中函数指针定义.
2. 子类函数指针强制转换为基类函数指针, 保存在父类提供的容器中.
3. 使用保存的成员函数指针调用函数.# C++ 代码整理

