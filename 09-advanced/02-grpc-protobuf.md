# 🔥 gRPC & Protocol Buffers

---

## 1. What is gRPC?

A high-performance RPC (Remote Procedure Call) framework by Google. Faster than REST for service-to-service communication.

```
REST: JSON over HTTP/1.1 (text-based)
gRPC: Protobuf over HTTP/2 (binary, fast)
```

---

## 2. Protocol Buffers (Protobuf)

```protobuf
// user.proto
syntax = "proto3";

message User {
  int32 id = 1;
  string name = 2;
  string email = 3;
}

service UserService {
  rpc GetUser (UserRequest) returns (User);
  rpc ListUsers (Empty) returns (stream User);
}
```

---

## 3. gRPC vs REST

| Feature | REST | gRPC |
|---------|------|------|
| Format | JSON | Protobuf (binary) |
| Protocol | HTTP/1.1 | HTTP/2 |
| Speed | Slower | Faster |
| Streaming | Limited | Native |
| Browser | Yes | Limited |
| Use case | External APIs | Internal services |

---

## 4. When to Use gRPC

```
✅ Use gRPC:
├── Service-to-service (microservices)
├── Low-latency requirements
├── Streaming data
└── Mobile clients (smaller payload)

✅ Use REST:
├── Public APIs
├── Browser clients
├── Simple CRUD
└── Human debugging needed
```

---

## 5. Quick Summary

```
gRPC: High-performance RPC
Protobuf: Binary serialization format

Benefits:
├── 10x faster than JSON
├── HTTP/2 streaming
├── Strong typing
└── Auto-generated code

Use for: Microservices, internal APIs
```

---

*Next: [Message Queues](./03-message-queues.md)*
