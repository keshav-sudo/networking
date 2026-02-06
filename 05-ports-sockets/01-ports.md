# 🚪 Ports - Gateway to Applications

---

## 1. Simple Explanation (Beginner Level)

IP address gets data to the right **computer**. But a computer runs many programs - how does data reach the right **program**?

**Ports!** Each application listens on a specific port number.

```
IP Address = Building Address (which building)
Port Number = Apartment Number (which apartment in building)

192.168.1.10:80   → Web Server
192.168.1.10:22   → SSH
192.168.1.10:3306 → MySQL
```

---

## 2. Real-World Analogy

```
Office Building (IP: 192.168.1.10):

Door 22:   Security Office (SSH)
Door 80:   Reception (HTTP)
Door 443:  VIP Reception (HTTPS)
Door 3000: Developer Lounge (Your App)
Door 27017: Database Room (MongoDB)

Visitor says: "I want building 192.168.1.10, door 443"
              → Goes directly to VIP Reception
```

---

## 3. Technical Working

### Port Number Ranges

```
0 - 1023:      Well-Known Ports (System)
               Need root/admin to use
               HTTP (80), HTTPS (443), SSH (22)

1024 - 49151:  Registered Ports
               Applications register these
               MySQL (3306), PostgreSQL (5432)

49152 - 65535: Dynamic/Ephemeral Ports
               OS assigns for outgoing connections
               Your browser uses these as source ports
```

### Common Ports Reference

| Port | Protocol | Service |
|------|----------|---------|
| 20, 21 | TCP | FTP (data, control) |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP (email send) |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 (email receive) |
| 143 | TCP | IMAP |
| 443 | TCP | HTTPS |
| 3000 | TCP | Common dev port |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |
| 6379 | TCP | Redis |
| 8080 | TCP | HTTP alt/proxy |
| 27017 | TCP | MongoDB |

### How Connection Uses Ports

```
Your Browser                          Web Server
(192.168.1.10)                       (93.184.216.34)

┌─────────────────┐                 ┌─────────────────┐
│ Firefox:52431   │────────────────►│ nginx:443       │
│ (ephemeral)     │   HTTPS conn    │ (well-known)    │
└─────────────────┘                 └─────────────────┘

Connection identified by 5-tuple:
(Protocol, SrcIP, SrcPort, DstIP, DstPort)
(TCP, 192.168.1.10, 52431, 93.184.216.34, 443)
```

---

## 4. Where Used in Real Systems?

| Context | Ports Used |
|---------|------------|
| Web dev | 3000, 8080 (dev), 80/443 (prod) |
| Databases |3306, 5432, 27017, 6379 |
| DevOps | 22 (SSH), 9090 (Prometheus) |
| Docker | Exposed ports map to host |

---

## 5. Practical Example

### Check What's Using a Port

```bash
# Mac/Linux - what's on port 3000?
lsof -i :3000

# Or use netstat
netstat -tulpn | grep 3000

# Kill process on port
kill $(lsof -t -i:3000)
```

### Node.js Server on Custom Port

```javascript
const express = require('express');
const app = express();

const PORT = process.env.PORT || 3000;  // Configurable

app.listen(PORT, '0.0.0.0', () => {
  console.log(`Server on port ${PORT}`);
  // 0.0.0.0 = listen on all interfaces
  // 127.0.0.1 = only localhost
});
```

### Docker Port Mapping

```yaml
# docker-compose.yml
services:
  web:
    image: nginx
    ports:
      - "8080:80"  # host:container
      # Access via localhost:8080
      # Container nginx runs on 80
  
  db:
    image: postgres
    ports:
      - "5432:5432"
```

---

## 6. Common Mistakes

| ❌ Mistake | ✅ Correct |
|-----------|-----------|
| "Port 80 = HTTP always" | Any protocol can run on any port |
| "Each port = one connection" | Port can handle thousands of connections |
| "Ports 0-1024 are blocked" | Need root, not blocked |

---

## 7. Quick Summary

```
Port: 16-bit number (0-65535) identifying application

Ranges:
- 0-1023: Well-known (HTTP 80, SSH 22)
- 1024-49151: Registered (MySQL 3306)
- 49152-65535: Ephemeral (client connections)

Connection = (Protocol, SrcIP, SrcPort, DstIP, DstPort)

Server: Listens on fixed port (e.g., 443)
Client: Uses random ephemeral port (e.g., 52431)
```

---

## 8. Quiz Questions

1. Why can your laptop have 100 tabs open to google.com:443?
2. Running `node app.js` fails with "EADDRINUSE". What happened?
3. `localhost:3000` works, but `192.168.1.10:3000` doesn't. Why?
4. Can two apps listen on same port?
5. How many total connections can a single server port handle?

---

## 9. Answer Key

<details>
<summary>Click to reveal</summary>

1. **Different source ports!** Each tab uses unique ephemeral port:
   - Tab 1: Your-IP:50001 → google:443
   - Tab 2: Your-IP:50002 → google:443
   - Each is separate connection

2. **Port already in use!** Another process is listening on that port. Find it with `lsof -i :3000` and kill it.

3. **Server bound to 127.0.0.1** (localhost only). Change to `0.0.0.0` to listen on all interfaces.

4. **No!** Only one process can bind to a port (unless using SO_REUSEPORT for load balancing). Different IPs on same port is OK.

5. **Theoretically: ~65k connections per client IP.** But limited by file descriptors, memory. Practical limit often 10k-100k with tuning. Server can handle millions total (different client IPs).

</details>

---

*Next: [Socket Programming](./03-socket-programming.md)*
