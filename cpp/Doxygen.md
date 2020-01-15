# Doxygen
---

## 常用关键字
| 关键字 | 说明 |
| -- | -- |
| @file  | 档案的批注说明 |
| @author | 作者的信息 |
| @brief | 用于class 或function的简易说明 |
| @param | 主要用于函数说明中，后面接参数的名字，然后再接关于该参数的说明 |
| @return | 描述该函数的返回值情况 |
| @retval | 描述返回值类型eg:@retval NULL 空字符串。@retval !NULL 非空字符串。 |
| @note | 注解 |
| @attention | 注意 |
| @warning | 警告信息 |
| @enum | 引用了某个枚举，Doxygen会在该枚举处产生一个链接eg：@enumCTest::MyEnum |
| @var | 引用了某个变量，Doxygen会在该枚举处产生一个链接eg：@varCTest::m_FileKey |
| @class | 引用某个类，格式：@class <name> [<header-file>] [<header-ame>]eg:@class CTest "inc/class.h" |
| @exception | 可能产生的异常描述eg:@exception 本函数执行可能会产生超出范围的异常 |

## 函数定义格式

/**
* @brief $end$
* 
* @param : $MethodArg$
* @return : $SymbolType$
* @author CJie
* @date $DATE$
* @remark 1.$DATE$ $HOUR$:$MINUTE$ created by CJie
* @See
*/
