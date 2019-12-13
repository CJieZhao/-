# Vcpkg C/C++ 开源库工具使用说明

----

## 安装（两种方式）

1. 访问 [官网](https://github.com/microsoft/vcpkg)
2. git下载 `git clone https://github.com/microsoft/vcpkg`

## 编译Vcpkg

使用Powershell，运行目录下`bootstrap-vcpkg.bat`

## 使用

### 查看支持开源库

`.\vcpkg.exe search`

### 安装开源库

`.\vcpkg.exe install jsoncpp`

### 编译指定平台

`.\vcpkg.exe install jsoncpp:x64-windows`

| 平台               |
| :----------------- |
| arm-uwp            |
| arm-windows        |
| arm64-uwp          |
| arm64-windows      |
| x64-uwp            |
| x64-windows-static |
| x64-windows        |
| x86-uwp            |
| x86-windows-static |
| x86-windows        |

### 移除编译过的开源库

`.\vcpkg.exe remove jsoncpp`

移除全部过时的库
`.\vcpkg.exe remove --outdated`

### 列出已经安装的开源库

`.\vcpkg.exe list`

[参考](https://blog.csdn.net/cjmqas/article/details/79282847)