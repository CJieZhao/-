C++ 协变
---

C++ 在设计接口函数返回类时无法知道未来子类类型，返回结果使用返回基类指针。在子类中调用接口函数时对基类指针的处理存在两种方式:

1. 对接口返回的基类实现所有后面子类需要的接口，这种处理方式只能在父类完全可以预知子类需要的接口函数情况，实际使用中很难达到。
2. 对接口返回的基类指针转换为相应的子类指针使用，这种处理方式需要调用方明确知道接口返回的实际是哪个子类指针。增加了调用者的额外工作，如果转换错误对程序可能有致命影响。

这种设计情况在实际中却是非常常用，C++ 返回值协变主要为了解决这种问题。

C++协变为子类继承的接口返回值类型可以使用符合里氏代换原则的类型进行重载。

例子：

``` Cpp

class AParam
{
public:
};

class BParam : public AParam
{
public:
};

class AClass
{
public:
    virtual AParam* GetParam() const
    {
        return nullptr;
    }
};

class BClass : public AClass
{
public:
    BParam* GetParam() const override    // 重载了基类中的GetParam接口，并使用了协变对函数返回值进行了替换。
    {
        return nullptr;
    }
};

```