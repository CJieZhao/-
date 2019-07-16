# GRPC

---

## Protobuf 中 GRPC 语法

包括原生 `Protobuf` 和 `GRPC` 扩展

```
// 标记 Protobuf 语法版本
syntax = "proto3";

// 引用其他 proto文件
import "Common.proto";

// 包名
package RPCProto;

// 定义结构
message ProjectInfo {
	string ProjectID = 1;
	string ProjectName = 2;
	string FullName = 3;

	string Factory = 4;
	string Department = 5;
	string Block = 6;
	int32 ReservoirType = 7;

	string CreateTime = 8;
}

message GetProjectResponse {
	ERRORTYPE state = 1;
	// 数组定义方式
	repeated ProjectInfo projects = 2;
}

message GetProjectRequest {
	int64 ID = 1;
}

// 定义RPC接口[GRPC]
service ProjectService {
	rpc GetPublicProjectList(GetProjectRequest) returns (GetProjectResponse) {}
}
```

## 从 Protobuf 生成代码脚本

如下脚本支持从原生 `Protobuf` 和 `GRPC`扩展， 生成 `C++` `C#` `Golang`的代码

```
@echo off

SETLOCAL enabledelayedexpansion

SET "PROTOPATH=../src/RPCProto"
SET ALLFILE=

REM FOR /R %PROTOPATH% %%i in (*.proto) do (
	REM ECHO %%i
REM	SET "ALLFILE=!ALLFILE! %%i")

for /f "delims=\" %%a in ('dir /b /a-d /o-d "%PROTOPATH%\*.proto"') do (
	SET "ALLFILE=!ALLFILE! %PROTOPATH%/%%a"
)

SET "CMD0=--proto_path=%PROTOPATH% --plugin=protoc-gen-grpc=.\grpc_cpp_plugin.exe --grpc_out=%PROTOPATH% %ALLFILE%"
SET "CMD1=--proto_path=%PROTOPATH% --plugin=protoc-gen-grpc=.\grpc_csharp_plugin.exe --grpc_out=%PROTOPATH% %ALLFILE%"
SET "CMD2=--proto_path=%PROTOPATH% --go_out=plugins=grpc:%PROTOPATH% --cpp_out=%PROTOPATH% %ALLFILE%"

REM ECHO %CMD0%
REM ECHO %CMD1%
REM ECHO %CMD2%

protoc.exe %CMD0%
protoc.exe %CMD1%
protoc.exe %CMD2%

@pause
```

## GRPC 使用 TLS 证书进行访问

###  使用 `Openssl` 生成证书文件

1. 生成私钥(`server.key`)

    `openssl genrsa -out server.key 2048`，生成 `RSA` 私钥， 其中 `server.key` 为输出文件, 2048 为密钥位数
    `openssl ecparam -genkey -name secp384r1 -out server.key`， 生成 `ECC` 私钥，命令为椭圆曲线密钥参数生成及操作，本文中`ECC`曲线选择的是`secp384r1`

1. 生成公钥(`server.pem`)

    `openssl req -new -x509 -sha256 -key server.key -out server.pem -days 3650`，其中 `-new` 创建新的证书，`-x509` 证书格式，`-sha256` , `-key` 私钥文件，`-out` 证书文件，`-days` 有效时间。 （此后则输入证书拥有者信息， 其中`Common Name`在客户端使用的时候需要用到）

    ```
    Country Name (2 letter code) [XX]:
    State or Province Name (full name) []:
    Locality Name (eg, city) [Default City]:
    Organization Name (eg, company) [Default Company Ltd]:
    Organizational Unit Name (eg, section) []:
    Common Name (eg, your name or your server's hostname) []:grpc server name
    Email Address []:
    ```

###  Golang 使用TLS证书

1. Golang 服务端加入证书

```
// 公钥， 私钥
c, err := credentials.NewServerTLSFromFile("server.pem", "server.key")
if err != nil {
    log.Fatalf("credentials.NewServerTLSFromFile err: %v", err)
}

server := grpc.NewServer(grpc.Creds(c))
```

1. Golang 客户端使用证书

```
// 公钥， Common Name
c, err := credentials.NewClientTLSFromFile("../../conf/server.pem", "grpc server name")
if err != nil {
    log.Fatalf("credentials.NewClientTLSFromFile err: %v", err)
}

conn, err := grpc.Dial(":"+PORT, grpc.WithTransportCredentials(c))
```

### C++ 使用TLS证书
