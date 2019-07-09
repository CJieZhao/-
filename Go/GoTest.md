# GoTest

---

## 测试说明

1. 测试文件命名 `*_test.go`
1. 必须引用testing包 `import testing`

### 单元测试

1. 函数命名要求：：`func TestXxx (t *testing.T) {}` Xxx部分可以为任意的字母数字的组合，但是首字母不能是小写字母[a-z]，例如Testintdiv是错误的函数名

    * 例如 ：`func TestLogin(t *testing.T) {}`

1. 测试运行错误：函数中通过调用testing.T的Error, Errorf, FailNow, Fatal, FatalIf方法，说明测试不通过，调用Log方法用来记录测试的信息。

    * 例如 ：`t.Error("Login Error")`

### 压力测试

1. 函数命名要求：压力测试用例必须遵循如下格式，其中XXX可以是任意字母数字的组合，但是首字母不能是小写字母

    * `func BenchmarkXXX(b *testing.B) { ... }`

1. 函数内部使用testing.B.N作为压力测试的循环次数

    * 例如 `for i := 0; i < b.N; i++ {}`

1. 执行命令 `go.exe test -test.bench=.* -run=none -benchtime=30s`

    * `-test.bench=.*` : 测试全部的压力测试函数
    * `-run=none` : 不运行单元测试函数
    * `-benchtime=30s`：性能测试运行的时间，默认是1s。t.N 最终会记录运行到指定时间的次数


## 测试例子

1. 单元测试

```
func Test_Division_1(t *testing.T) {
    if i, e := Division(6, 2); i != 3 || e != nil { //try a unit test on function
        t.Error("除法函数测试没通过") // 如果不是如预期的那么就报错
    } else {
        t.Log("第一个测试通过了") //记录一些你期望记录的信息
    }
}

func Test_Division_2(t *testing.T) {
    t.Error("测试不通过")
}
```

2. 压力测试

```
package gotest

import (
    "testing"
)

func Benchmark_Division(b *testing.B) {
    for i := 0; i < b.N; i++ { //use b.N for looping 
        Division(4, 5)
    }
}

func Benchmark_TimeConsumingFunction(b *testing.B) {
    b.StopTimer() //调用该函数停止压力测试的时间计数

    //做一些初始化的工作,例如读取文件数据,数据库连接之类的,
    //这样这些时间不影响我们测试函数本身的性能

    b.StartTimer() //重新开始时间
    for i := 0; i < b.N; i++ {
        Division(4, 5)
    }
}
```