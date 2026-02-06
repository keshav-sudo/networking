# 📶 TCP Protocol Deep Dive

---

## 1. Simple Explanation (Beginner Level)

**TCP = Transmission Control Protocol**

TCP is like **registered mail with tracking**: 
- Guaranteed delivery
- Arrives in correct order
- Confirmation that it was received

If a packet gets lost, TCP automatically resends it!

---

## 2. Real-World Analogy

```
TCP = Phone Call:
1. Dial and wait for answer (handshake)
2. "Hello?" "Hi, can you hear me?" "Yes" (3-way)
3. Have conversation (data transfer)
4. "Bye" "Bye" (connection close)

UDP = Shouting across street:
Just shout and hope they hear!
```

---

## 3. Technical Working

### TCP 3-Way Handshake

```
Client                          Server
   │                               │
   │ ─── SYN (Seq=100) ──────────►│
   │     "Can we talk?"            │
   │                               │
   │ ◄── SYN-ACK (Seq=300,Ack=101)─│
   │     "Yes, can you hear me?"   │
   │                               │
   │ ─── ACK (Ack=301) ──────────►│
   │     "Yes, let's talk!"        │
   │                               │
   │ ════ CONNECTION ESTABLISHED ══│
```

### TCP Header

```
┌─────────────────────────────────────────────┐
│ Source Port (16)   │ Dest Port (16)         │
├─────────────────────────────────────────────┤
│           Sequence Number (32)              │
├─────────────────────────────────────────────┤
│        Acknowledgment Number (32)           │
├───────────────┬─────────────────────────────┤
│ Offset│Flags  │     Window Size (16)        │
├───────────────┴─────────────────────────────┤
│ Checksum (16)      │ Urgent Pointer (16)    │
└─────────────────────────────────────────────┘

Important Flags:
SYN - Start connection
ACK - Acknowledge
FIN - Finish connection  
RST - Reset/abort
PSH - Push data immediately
```

### Reliability Mechanism

```
Sender                        Receiver
   │                              │
   │ ─ Seq 1: "Hello" ──────────►│
   │                              │
   │ ◄──────────── ACK 6 ─────────│ (Got 5 bytes)
   │                              │
   │ ─ Seq 6: "World" ──────────►│
   │                              │
   │ ◄──────────── ACK 11 ────────│
   │                              │

If ACK not received → Resend!
```

### Connection Close (4-Way)

```
Client                          Server
   │ ─── FIN ─────────────────────►│
   │ ◄─── ACK ─────────────────────│
   │ ◄─── FIN ─────────────────────│
   │ ─── ACK ─────────────────────►│
   │ (TIME_WAIT for ~60 sec)      │
```

---

## 4. Where Used?

All reliable communication: HTTP, HTTPS, SSH, FTP, SMTP, database connections.

---

## 5. Practical Example

```javascript
// TCP Server in Node.js
const net = require('net');

const server = net.createServer((socket) => {
  console.log('Client connected');
  
  socket.on('data', (data) => {
    console.log('Received:', data.toString());
    socket.write('ACK: ' + data);
  });
  
  socket.on('end', () => console.log('Client disconnected'));
});

server.listen(9000);
```

```bash
# View TCP connections
netstat -an | grep tcp

# See TCP states
ss -tan state established
```

---

## 6. Quick Summary

```
TCP: Reliable, ordered, connection-oriented

3-Way Handshake: SYN → SYN-ACK → ACK
4-Way Close: FIN → ACK → FIN → ACK

Features:
- Sequence numbers (ordering)
- ACKs (confirmation)
- Retransmission (reliability)
- Flow control (don't overwhelm)
```

---

## 7. Quiz

1. Why 3-way handshake, not 2-way?
2. What's TIME_WAIT and why does it exist?
3. Server not responding, client sends SYN. What happens?

<details>
<summary>Answers</summary>

1. 2-way only confirms one direction. 3-way confirms BOTH sides can send AND receive.

2. TIME_WAIT (~60s): Ensures old packets don't interfere with new connections on same port. Also allows delayed ACKs to arrive.

3. TCP retries SYN with exponential backoff (1s, 2s, 4s...). Eventually gives up with connection timeout.
</details>

---

*Next: [UDP Protocol](./02-udp-protocol.md)*
