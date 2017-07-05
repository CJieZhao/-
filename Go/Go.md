# GO
---
## 1. 类
```
package main  
  
import "fmt"  
  
type data struct {  
  
    val int  
}  
  
  
func (p_data* data)set(num int) {  
  
    p_data.val = num  
}  
  
  
func (p_data* data)show() {  
  
    fmt.Println(p_data.val)  
}  
  
  
func main() {  
  
    p_data := &data{4}  
    p_data.set(5)  
    p_data.show()  
  
}  
```

## 2. 继承
```
package main  
  
import "fmt"  
  
type parent struct {  
  
    val int  
}  
  
type child struct {  
  
    parent  
    num int  
}  
  
  
func main() {  
  
    var c child  
  
    c = child{parent{1}, 2}  
    fmt.Println(c.num)  
    fmt.Println(c.val)  
  
} 
```

## 3. 多态
```
package main  
  
import "fmt"  
  
type act interface {  
  
    write()  
}  
  
type xiaoming struct {  
  
}  
  
type xiaofang struct {  
  
}  
  
func (xm *xiaoming) write() {  
  
    fmt.Println("xiaoming write")  
}  
  
func (xf *xiaofang) write() {  
      
    fmt.Println("xiaofang write")  
}  
  
func main() {  
  
    var w act;  
  
    xm := xiaoming{}  
    xf := xiaofang{}  
      
    w = &xm  
    w.write()  
  
    w = &xf  
    w.write()  
}
```
