# 🔗 Socket Programming

---

## 1. What is a Socket?

A **socket** is an endpoint for network communication - the combination of IP address + Port.

```
Socket = IP:Port
Example: 192.168.1.10:3000

Connection = Two sockets talking
Client socket → Server socket
```

---

## 2. Socket Types

```
STREAM Socket (TCP):
└── Reliable, ordered, connection-based
└── Like a phone call

DATAGRAM Socket (UDP):
└── Unreliable, no connection
└── Like sending postcards
```

---

## 3. TCP Socket Flow

```
Server:                    Client:
                          
socket()                   socket()
   │                          │
bind(port)                    │
   │                          │
listen()                      │
   │                          │
accept() ◄─────────────── connect()
   │                          │
recv() ◄───────────────── send()
   │                          │
send() ────────────────► recv()
   │                          │
close()                   close()
```

---

## 4. Code Example (Node.js TCP)

```javascript
// Server
const net = require('net');

const server = net.createServer((socket) => {
  console.log('Client connected');
  
  socket.on('data', (data) => {
    console.log('Received:', data.toString());
    socket.write('Echo: ' + data);
  });
  
  socket.on('close', () => {
    console.log('Client disconnected');
  });
});

server.listen(3000, '0.0.0.0', () => {
  console.log('Server on port 3000');
});
```

```javascript
// Client
const net = require('net');

const client = net.createConnection({ port: 3000 }, () => {
  console.log('Connected');
  client.write('Hello Server!');
});

client.on('data', (data) => {
  console.log('Reply:', data.toString());
  client.end();
});
```

---

## 5. Common Socket Options

```javascript
// Allow port reuse
server.on('listening', () => {
  server.setKeepAlive(true, 60000);  // Keep connection alive
});

// Important for production
server.maxConnections = 1000;
server.timeout = 30000;  // 30 sec timeout
```

---

## 6. Quick Summary

```
Socket: IP + Port endpoint
Connection: Two sockets linked

TCP Sockets: Reliable stream
UDP Sockets: Fast datagrams

Flow: socket() → bind() → listen() → accept() → send/recv → close()

Key syscalls: socket, bind, listen, accept, connect, send, recv, close
```

---

*Next: [Network Debugging](./04-network-debugging.md)*
