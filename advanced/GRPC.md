# 🧮 gRPC

gRPC is an RPC (Remote Procedure Call) framework that was developed by Google originally and then made open source and maintained by the community. This one is multi-platform and allows communicating from one server with another in a bidirectional way thanks to the HTTP2 protocol. This one also uses protobuf for data serialization (the body is converted to binary for more 
performance)


https://grpc.io/docs/what-is-grpc/introduction/


![Image](https://grpc.io/img/landing-2.svg)


## 🤖Protobuf (Protocol Buffers)  

Allows efficiently converting complex objects to binary format which makes it super efficient. 

Its functioning you will have to describe an interface in a .proto file and based on that various libraries will be able to generate the model and the client for it. 

```protobuf
message Person {
  string name = 1;
  int32 id = 2;  // Unique ID number for this person.
  string email = 3;

  enum PhoneType {
    PHONE_TYPE_UNSPECIFIED = 0;
    PHONE_TYPE_MOBILE = 1;
    PHONE_TYPE_HOME = 2;
    PHONE_TYPE_WORK = 3;
  }

  message PhoneNumber {
    string number = 1;
    PhoneType type = 2;
  }

  repeated PhoneNumber phones = 4;

  google.protobuf.Timestamp last_updated = 5;
}

// Our address book file is just one of these.
message AddressBook {
  repeated Person people = 1;
}
```


To easily recognize which object we use protobuf will add a tag to each field to facilitate the mapping which makes it extensible and will not break the old tags if we add new ones


