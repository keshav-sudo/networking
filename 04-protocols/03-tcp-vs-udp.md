# ⚔️ TCP vs UDP - Choosing the Right Protocol

---

## 1. Simple Comparison

| Feature | TCP | UDP |
|---------|-----|-----|
| **Full Name** | Transmission Control Protocol | User Datagram Protocol |
| **Connection** | Connection-oriented | Connectionless |
| **Reliability** | Guaranteed delivery | Best effort |
| **Ordering** | Maintains order | No ordering |
| **Speed** | Slower (overhead) | Faster |
| **Header Size** | 20-60 bytes | 8 bytes |

---

## 2. Analogy

```
TCP = Phone Call:
📞 Ring ring → Hello? → Hi! → [conversation] → Bye → Bye
✓ Know other person is there
✓ Can ask to repeat
✓ Orderly conversation

UDP = Walkie-Talkie broadcast:
📻 "Message for everyone!" [no confirmation]
✓ Fast
✓ One-to-many easy
✗ Don't know who heard it
```

---

## 3. When to Use What?

### Use TCP When:
- **Data integrity matters** (files, emails, web pages)
- **Order matters** (database transactions)
- **Delivery confirmation needed** (API calls)

```
Examples: HTTP/HTTPS, SSH, FTP, SMTP, Database connections
```

### Use UDP When:
- **Speed > reliability** (real-time apps)
- **Lost data is acceptable** (video frames)
- **Broadcasting/multicasting** (service discovery)

```
Examples: DNS, VoIP, Video streaming, Gaming, DHCP
```

---

## 4. Technical Comparison

```
TCP Packet:
┌──────────────────────────────────────────────────┐
│ Src Port │ Dst Port │ Seq # │ Ack # │ Flags... │
│ Window │ Checksum │ Options │ DATA              │
└──────────────────────────────────────────────────┘
20-60 bytes header + error recovery + flow control

UDP Packet:
┌────────────────────────────────────────┐
│ Src Port │ Dst Port │ Length │ Check │ DATA
└────────────────────────────────────────┘
8 bytes header only! Fire and forget.
```

---

## 5. Real-World Scenarios

| Scenario | Protocol | Why? |
|----------|----------|------|
| Download file | TCP | Need every byte, in order |
| Live video | UDP | Skip lost frame, show current |
| Web browsing | TCP | Pages must load correctly |
| DNS query | UDP | Quick lookup, retry if lost |
| Online game | UDP | Fast player positions matter |
| Bank transaction | TCP | Must be reliable! |
| Voice call | UDP | Slight drops OK, low latency critical |

---

## 6. Code Example

```javascript
// TCP Server (reliable)
const net = require('net');
net.createServer((socket) => {
  socket.on('data', (data) => socket.write('ACK'));
}).listen(9000);

// UDP Server (fast)  
const dgram = require('dgram');
const server = dgram.createSocket('udp4');
server.on('message', (msg) => console.log('Got:', msg));
server.bind(9000);
```

---

## 7. Quick Summary

```
TCP: Reliable but slower
- Use for: Web, APIs, Files, Databases, Email

UDP: Fast but unreliable
- Use for: Video, Voice, Gaming, DNS, IoT

Decision:
├── Need reliability? → TCP
├── Need speed? → UDP
└── Need both? → QUIC (HTTP/3) or implement ACK on UDP
```

---

## 8. Quiz

1. WhatsApp messages use TCP or UDP?
2. YouTube live streaming: TCP or UDP?
3. DNS uses UDP but falls back to TCP when? Why?

<details>
<summary>Answers</summary>

1. **TCP**: Messages must arrive, in order, reliably. Can't lose a message!

2. **Both!** Video chunks over UDP for speed, but control/buffering may use TCP. YouTube often uses TCP anyway.

3. **When response > 512 bytes** (truncated). TCP for zone transfers. UDP is limited to smaller responses.
</details>

---

*Next: [HTTP/HTTPS Deep Dive](./04-http-https.md)*
