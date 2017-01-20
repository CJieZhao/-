# zlib 用法
---
## 如何获得zlib
zlib的主页是:http://www.zlib.net/

## 用zlib能干什么
先来看看 zlib 都提供了那些函数, 都在zlib.h中,看到一堆宏不要晕,其实都是为了兼容各种编译器和一些类型定义.死死抓住那些主要的函数的原型声明就不会受到这些东西的影响了.
关键的函数有那么几个:

* (1)`int compress (Bytef *dest,   uLongf *destLen, const Bytef *source, uLong sourceLen);`
把源缓冲压缩成目的缓冲, 就那么简单, 一个函数搞定
* (2)`int compress2 (Bytef *dest,   uLongf *destLen,const Bytef *source, uLong sourceLen,int level);`
功能和上一个函数一样,都一个参数可以指定压缩质量和压缩数度之间的关系(0-9)不敢肯定这个参数的话不用太在意它,明白一个道理就好了: 要想得到高的压缩比就要多花时间
* (3)`uLong compressBound (uLong sourceLen);`
计算需要的缓冲区长度. 假设你在压缩之前就想知道你的产度为 sourcelen 的数据压缩后有多大, 可调用这个函数计算一下,这个函数并不能得到精确的结果,但是它可以保证实际输出长度肯定小于它计算出来的长度
* (4)`int uncompress (Bytef *dest,   uLongf *destLen,const Bytef *source, uLong sourceLen);`解压缩(看名字就知道了:)
* (5) deflateInit() + deflate() + deflateEnd()
3个函数结合使用完成压缩功能,具体用法看 example.c 的 test_deflate()函数. 其实 compress() 函数内部就是用这3个函数实现的(工程 zlib 的 compress.c 文件)
* (6) `inflateInit() + inflate() + inflateEnd()`和(5)类似,完成解压缩功能.
* (7) gz开头的函数. 用来操作*.gz的文件,和文件stdio调用方式类似. 想知道怎么用的话看example.c 的 test_gzio() 函数,很easy.

总结: 其实只要有了compress() 和uncompress() 两个函数,在大多数应用中就足够了.

## 测试
* 在工程中加入动态链接库或者静态链接库，需要修改函数的调用约定_stdcall。

```
#ifdef __cplusplus
extern "C"
{
#include "zlib.h"
#include "zconf.h"
}
#endif

#include "iostream"
#include <windows.h>

//#define ZLIB_WINAPI

struct TestSt
{
	int t1;
	int t2;
	float t3;
	float t4;
	char tmp[256];
};

#pragma comment(lib, "zlib.lib")

int _cdecl main()
{
	unsigned char dest[1024] = {0};
	unsigned long destSize = sizeof(dest);

	unsigned char undest[1024] = {0};
	unsigned long undestSize = sizeof(undest);

	TestSt st = {0};
	st.t1 = 0;
	st.t2 = 0;
	st.t3 = 0.1f;
	st.t4 = 10.0f;
	strcpy(st.tmp, "fsdfsdf");

	//const unsigned char src[] = "test zlib dfjkhdakjfkasjdfhkasjdfhkajdfhlkafkjdslfkjasfjkd la 123123123123";
	unsigned long srcSize = sizeof(TestSt);

	std::cout << srcSize << std::endl;

	//HMODULE hMod = LoadLibraryA("zlibwapi.dll");

	//typedef int (*compressF)OF((Bytef *dest, uLongf *destLen, const Bytef *source, uLong sourceLen, int level));
	//typedef int (*uncompressF)OF((Bytef *dest, uLongf *destLen, const Bytef *source, uLong sourceLen));

	//compressF compress2 = (compressF)GetProcAddress(hMod, "compress2");
	//uncompressF uncompress = (uncompressF)GetProcAddress(hMod, "uncompress");

	if (Z_OK != compress2(dest, &destSize, (const Bytef *)&st, srcSize, 9))
	{
		std::cout << "压缩失败!" << std::endl;
	}

	if (Z_OK != uncompress(undest, &undestSize, dest, destSize))
	{
		std::cout << "解压失败!" << std::endl;
	}

	TestSt sts = {0};
	memcpy(&sts, undest, sizeof(TestSt));

	return 0;
}
```



