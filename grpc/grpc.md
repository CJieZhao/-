# GRPC
---

## 从PB生成代码指令

```
protoc.exe -I=. --grpc_out=. --plugin=protoc-gen-grpc=.\grpc_cpp_plugin.exe helloworld.proto
protoc.exe -I=. --cpp_out=. helloworld.proto
```