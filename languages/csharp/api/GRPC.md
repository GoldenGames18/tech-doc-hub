# 🧮gRCP

[explanation](../../../advanced/GRPC.md)


## 👨‍🏫Tuto

[https://learn.microsoft.com/en-us/aspnet/core/grpc/?view=aspnetcore-9.0](https://learn.microsoft.com/en-us/aspnet/core/grpc/?view=aspnetcore-9.0)

## 🧩Example of proto

```protobuf
syntax = "proto3";

option csharp_namespace = "GrpcService";

package greet;

// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply);
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings.
message HelloReply {
  string message = 1;
}
```

## 📚Libs

Server

```protobuf
dotnet add package Grpc.AspNetCore
dotnet add package Google.Protobuf
dotnet add package Grpc.Tools
```

Client:

```protobuf
dotnet add package Grpc.Net.Client
dotnet add package Google.Protobuf
dotnet add package Grpc.Tools
```


## ✨ Tips

The best practice is to make a project on the side that will contain all your definitions of your proto files and will implement will use the client and server libs which allows centralizing the storage of the proto files and their versioning thanks to the project. 

After it will be necessary to automate with a CI for the generation of your libs