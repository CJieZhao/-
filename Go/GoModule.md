# Go Module 内置的包管理工具
---

Golang 内部支持的包管理工具，支持代理。可以解决GRPC等库在dep下无法下载的问题

## 用法

1. 测试的版本  
    
    `go version go1.12.5 windows/amd64`

1. 命令格式

    ```
    Go mod provides access to operations on modules.

    Note that support for modules is built into all the go commands,
    not just 'go mod'. For example, day-to-day adding, removing, upgrading,
    and downgrading of dependencies should be done using 'go get'.
    See 'go help modules' for an overview of module functionality.

    Usage:

            go mod <command> [arguments]

    The commands are:

            download    download modules to local cache
            edit        edit go.mod from tools or scripts
            graph       print module requirement graph
            init        initialize new module in current directory
            tidy        add missing and remove unused modules
            vendor      make vendored copy of dependencies
            verify      verify dependencies have expected content
            why         explain why packages or modules are needed

    Use "go help mod <command>" for more information about a command.
    ```

1. go.mod 文件格式

    ```
    module github.com/my/thing

    require (
        // 包路径 版本号
        github.com/some/dependency v1.2.3
        github.com/another/dependency/v4 v4.0.0
        my/example/pkg v0.0.0
    )
    
    // 路径替换（可解决GFW导致无法下载的问题）
    replace my/example/pkg => github.com/example/pkg v0.0.0
    ```

1. 相关的环境变量
    
    1. GO111MODULE 是否使用 GOPATH。可选参数有    `auto`, `on`,`off`。
    1. GOPROXY 依赖包下载代理，使用代理下载可以不用路径替换。目前支持的有 [goproxy](https://goproxy.io)

## 测试例子

1. 测试环境 Windows10，测试路径  `F:\zcj\tmp\TestGO\Main.go`

1. 初始化 package 名为 __TestGo__ 的项目, 命令成功后会生成 `go.mod` 文件

    ```
    > go mod init TestGo
    ```
1. 下载依赖项

    ```
    > go mod download
    ```
