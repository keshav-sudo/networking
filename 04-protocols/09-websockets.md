# 🔌 WebSockets

---

## 1. Simple Explanation (Beginner Level)

Normal HTTP is like sending letters:
- You send request, wait for response, done
- Connection closes after each exchange
- To get updates, you have to keep asking ("Any new messages?")

**WebSocket is like a phone call:**
- You establish connection once
- Both sides can talk anytime
- Connection stays open until you hang up

Best for: Chat apps, live notifications, real-time games, stock tickers

---

## 2. Real-World Analogy

```
HTTP (Request-Response):
📱 → "Any new messages?" → 📧 Server
📱 ← "Nope" ← 📧
📱 → "Any new messages?" → 📧 (1 second later)
📱 ← "Nope" ← 📧
📱 → "Any new messages?" → 📧 (1 second later) 
📱 ← "Yes! Here's one" ← 📧
(Wasteful polling!)

WebSocket (Persistent):
📱 ←══════════════════════► 📧 Server
     (connection stays open)
📱 ← "New message!" ← 📧 (server pushes instantly)
📱 → "Send this" → 📧 (client can send anytime too)
```

---

## 3. Technical Working

### WebSocket Handshake (Upgrade from HTTP)

```
Client Request:
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket              ← "I want WebSocket"
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13

Server Response:
HTTP/1.1 101 Switching Protocols   ← "OK, switching"
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=

═══ Now it's WebSocket, not HTTP! ═══
```

### WebSocket vs HTTP Comparison

| Feature | HTTP | WebSocket |
|---------|------|-----------|
| Connection | New per request | Persistent |
| Direction | Client initiates | Bidirectional |
| Overhead | Headers every time | Minimal after handshake |
| Server Push | No (need polling) | Yes |
| Use Case | REST APIs, pages | Real-time apps |

### Connection Lifecycle

```
[Handshake] → [Open] → [Messages ↔] → [Close]
     │           │           │           │
   HTTP        WS          WS         Either
  Upgrade    onopen      onmessage    can close
```

---

## 4. Where Used in Real Systems?

| Application | Why WebSocket? |
|-------------|---------------|
| Slack/Discord | Real-time chat |
| Trading platforms | Live stock prices |
| Multiplayer games | Real-time positions |
| Collaborative docs (Google Docs) | Live updates |
| Notifications | Instant push |

---

## 5. Practical Example

### Server (Node.js with ws library)

```javascript
const WebSocket = require('ws');
const server = new WebSocket.Server({ port: 8080 });

server.on('connection', (socket) => {
  console.log('Client connected');
  
  // Receive message from client
  socket.on('message', (data) => {
    console.log('Received:', data.toString());
    
    // Broadcast to all clients
    server.clients.forEach((client) => {
      if (client.readyState === WebSocket.OPEN) {
        client.send(`Echo: ${data}`);
      }
    });
  });
  
  socket.on('close', () => {
    console.log('Client disconnected');
  });
  
  // Send welcome message
  socket.send('Welcome to the chat!');
});
```

### Client (Browser)

```javascript
const socket = new WebSocket('ws://localhost:8080');

socket.onopen = () => {
  console.log('Connected!');
  socket.send('Hello Server!');
};

socket.onmessage = (event) => {
  console.log('Received:', event.data);
};

socket.onclose = () => {
  console.log('Disconnected');
};

socket.onerror = (error) => {
  console.error('WebSocket Error:', error);
};
```

### Socket.IO (Popular Library)

```javascript
// Server
const io = require('socket.io')(3000);

io.on('connection', (socket) => {
  socket.on('chat message', (msg) => {
    io.emit('chat message', msg);  // Broadcast
  });
});

// Client
const socket = io('http://localhost:3000');
socket.emit('chat message', 'Hello!');
socket.on('chat message', (msg) => console.log(msg));
```

---

## 6. Common Mistakes

| ❌ Mistake | ✅ Correct |
|-----------|-----------|
| "Use WS for everything" | REST is fine for request-response; WS has overhead |
| "WS is more secure" | Same security as HTTP; use WSS (WS + TLS) |
| "No need for reconnection logic" | Connections drop! Implement auto-reconnect |

---

## 7. Quick Summary

```
WebSocket: Persistent, bidirectional connection

Flow: HTTP Upgrade → 101 Switching → WS connection

Use When:
- Real-time updates needed
- Server needs to push data
- Frequent bidirectional communication

Don't Use When:
- Simple request-response (use HTTP)
- Infrequent updates (use polling or SSE)

Remember:
- Starts as HTTP, upgrades to WS
- Both sides can send anytime
- Implement reconnection logic!
```

---

## 8. Quiz Questions

1. Your chat app uses WebSocket. Server restarts. What happens to connections?
2. Stock ticker updates every 100ms. HTTP polling or WebSocket? Why?
3. WebSocket connection url is `wss://chat.example.com`. What does `wss` mean?
4. Can WebSocket work through most firewalls?
5. User opens 10 browser tabs to your app. How many WS connections?

---

## 9. Answer Key

<details>
<summary>Click to reveal</summary>

1. **All connections drop!** Server restart = all clients disconnected. Need reconnection logic on client side.

2. **WebSocket**: 100ms polling = 10 requests/second = 600/minute = wasteful. WebSocket sends only when data changes.

3. **WebSocket Secure** (WS over TLS). Like HTTPS for WebSocket. Always use wss in production!

4. **Yes**, because handshake is HTTP. Most firewalls allow :80/:443. Some may still block upgraded connections though.

5. **10 connections!** Each tab = separate connection. Consider: token auth on each, shared state server-side.

</details>

---

*Next: [gRPC & Protobuf](../09-advanced/02-grpc-protobuf.md)*
