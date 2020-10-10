# C++ 多继承

在 c++ 中使用多继承，当涉及到父类指针转换`void*`时存在一个容易被忽略的偏移问题。

``` cpp
class IWorker
{
public:
    virtual void Do() = 0;
};

class IProgress
{
public:
    virtual unsigned int GetStep() const = 0;
    virtual void Step(unsigned int step) = 0;
};

class Progress : public IProgress
{
public:
    unsigned int GetStep() const
    {
        return _step.load(std::memory_order_relaxed);
    }

    void Step(unsigned int step)
    {
        _step.store(step % 100, std::memory_order_relaxed);
    }

private:
    std::atomic_uint _step = 0;
};

class W1 : public IWorker, public Progress
{
public:
    virtual void Do() override
    {
        int si = 10;
        for (int i = 0; i < 10; ++i) {
            Step(si += 5);

            printf("%d\n", GetStep());
        }
    }

private:
    int _value = 100;
    double _d = 200.0;
};

void testVirtualPtr(void *p)
{
    IProgress *ptr = (IProgress *)p;

    auto r = ptr->GetStep();

    return;
}

int main(int argc, char *argv[])
{
    W1 w;

    void *addrW = &w;
    printf("W1 address = %p\n", addrW);

    IProgress *p = nullptr;
    p = &w;
    printf("IProgress address = %p\n", p);

    IWorker *pW = nullptr;
    pW = &w;
    printf("IWorker address = %p\n", pW);

    return 0;
}
```

运行结果：

```
W1 address = 003DFBBC
IProgress address = 003DFBC0
IWorker address = 003DFBBC
```

在 `IProgress` 类型转换的时候，编译器对地址进行了偏移。在 `testVirtualPtr` 接口中 __编译器不会对参数 `p` 进行地址偏移__ 。
