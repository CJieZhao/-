# Golang 依赖管理工具 Dep

## 安装

```
go get -u github.com/golang/dep/cmd/dep
```

## 命令

```
$ dep
dep is a tool for managing dependencies for Go projects

Usage: dep <command>

Commands:

		init    Initialize a new project with manifest and lock files
	status  Report the status of the project's dependencies
	ensure  Ensure a dependency is safely vendored in the project
	prune   Prune the vendor tree of unused packages

Examples:
	dep init                               set up a new project
	dep ensure                             install the project's dependencies
	dep ensure -update                     update the locked versions of all dependencies
	dep ensure -add github.com/pkg/errors  add a dependency to the project

Use "dep help [command]" for more information about a command.
```

## 使用

1. 在代码中import需要的包， 例如：`import "github.com/gomodule/redigo"`
1. 进入项目目录执行 `dep init`
1. 初始化成功后执行 `dep ensure`
