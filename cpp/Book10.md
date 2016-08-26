# int 转 float 
===
``` cpp
int i = 32;					//1.
float f = *(float*)&i;		//2.
float g = (float)i;			//3.
```

1. int 型 内存分布为 二进制 直接转换为 十进制
2. float 型 内存分布为 [float & double 内存布局](Book11.md)（1.xxx * 2^n）
3. `*(float*)&i;` 这种转换会直接将int型内存布局当成是float型内存布局。最后结果`f != 32`
4. 而第二种转换则由编译器完成相应的位的转换。
5. 通过这种转换可以直接获取float型内存布局, 可以用来符号判断, 通过位运算可以进行多个float符号进行判断。