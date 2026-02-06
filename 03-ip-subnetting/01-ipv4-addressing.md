# 🔢 IPv4 Addressing

---

## 1. Simple Explanation (Beginner Level)

An **IP Address** is like a **home address** for every device on a network. Just like your house needs an address for mail delivery, every computer needs an IP address to receive data.

**IPv4** = Internet Protocol version 4 - Uses 4 numbers separated by dots.

Example: `192.168.1.10`

Each number ranges from 0-255, giving us about 4.3 billion possible addresses.

---

## 2. Real-World Analogy

```
IP Address = Home Address

192.168.1.10 is like:
├── 192      → Country
├── 168      → City  
├── 1        → Street
└── 10       → House Number

Network part: 192.168.1 (the street)
Host part:    .10 (specific house on that street)
```

---

## 3. Technical Working

### Binary Representation

```
IP: 192.168.1.10

Binary breakdown:
192 = 11000000
168 = 10101000
1   = 00000001
10  = 00001010

Full: 11000000.10101000.00000001.00001010

Total: 32 bits (4 bytes × 8 bits)
Max value per octet: 255 (11111111)
```

### IP Address Classes (Historical)

| Class | Range | Default Mask | Networks | Hosts/Network |
|-------|-------|--------------|----------|---------------|
| **A** | 1-126.x.x.x | 255.0.0.0 | 126 | 16 million |
| **B** | 128-191.x.x.x | 255.255.0.0 | 16,384 | 65,534 |
| **C** | 192-223.x.x.x | 255.255.255.0 | 2 million | 254 |
| **D** | 224-239.x.x.x | N/A | Multicast | - |
| **E** | 240-255.x.x.x | N/A | Reserved | - |

Note: 127.x.x.x is reserved for loopback (localhost).

### Private vs Public IP Ranges

```
PRIVATE (Not routable on internet):
┌────────────────────────────────────────┐
│ 10.0.0.0    - 10.255.255.255  (Class A)│
│ 172.16.0.0  - 172.31.255.255  (Class B)│
│ 192.168.0.0 - 192.168.255.255 (Class C)│
└────────────────────────────────────────┘

SPECIAL:
127.0.0.0 - 127.255.255.255  → Loopback (localhost)
169.254.x.x                  → Link-local (APIPA)
0.0.0.0                      → "Any" address
255.255.255.255              → Broadcast
```

### Network vs Host Portion

```
IP:     192.168.1.100
Mask:   255.255.255.0

Network portion: 192.168.1   (where mask is 255)
Host portion:    .100        (where mask is 0)

Same network: 192.168.1.1, 192.168.1.50, 192.168.1.200
Different:    192.168.2.1 (different network portion)
```

---

## 4. Where Used in Real Systems?

| Context | IP Usage |
|---------|----------|
| **localhost** | 127.0.0.1 - Your own machine |
| **Docker** | 172.17.0.x - Container network |
| **AWS VPC** | 10.0.0.0/16 - Private cloud network |
| **Home Router** | 192.168.1.1 - Gateway |
| **Public Server** | 52.x.x.x - Cloud instances |

---

## 5. Practical Example

```bash
# Check your IP
ip addr show          # Linux
ifconfig             # Mac
ipconfig             # Windows

# Find gateway
ip route | grep default

# Check if IP reachable
ping 8.8.8.8

# Trace route to destination
traceroute google.com
```

```javascript
// Node.js - Get local IP
const os = require('os');
const interfaces = os.networkInterfaces();

for (const name in interfaces) {
  for (const iface of interfaces[name]) {
    if (iface.family === 'IPv4' && !iface.internal) {
      console.log(`${name}: ${iface.address}`);
    }
  }
}
```

---

## 6. Common Mistakes

| ❌ Mistake | ✅ Correct |
|-----------|-----------|
| "Every device has public IP" | Most have private IP; NAT provides public access |
| "192.168.x.x works on internet" | Private IPs only work locally; need NAT |
| "IP identifies a device forever" | IPs can change (DHCP); MAC is permanent |

---

## 7. Quick Summary

```
IPv4: 32-bit address (4 octets, 0-255 each)
Example: 192.168.1.10

Private ranges:
  10.x.x.x, 172.16-31.x.x, 192.168.x.x

Special:
  127.0.0.1 = localhost
  0.0.0.0 = all interfaces
  255.255.255.255 = broadcast

Network part = street, Host part = house number
Subnet mask separates them
```

---

## 8. Quiz Questions

1. Is 192.168.5.10 a public or private IP? Can you access it from the internet?
2. What's the maximum value of each octet in IPv4? Why?
3. Your server listens on 127.0.0.1:3000. Can other computers access it?
4. Docker container has IP 172.17.0.5. Your host has 192.168.1.100. How do they communicate?
5. How many IP addresses exist in a /24 network?

---

## 9. Answer Key

<details>
<summary>Click to reveal</summary>

1. **Private IP** (192.168.x.x range). Cannot access from internet directly - needs NAT/port forwarding.

2. **255** because each octet is 8 bits, and 2^8 - 1 = 255 (11111111 in binary)

3. **No!** 127.0.0.1 is loopback - only accessible from the same machine. Use 0.0.0.0 to listen on all interfaces.

4. **Docker bridge network**: Docker creates virtual network. Host uses docker0 bridge interface to reach container. Container uses gateway to reach host.

5. **/24 = 256 addresses** (2^8). Usable hosts = 254 (first is network, last is broadcast).

</details>

---

*Next: [Subnet Masks](./02-subnet-masks.md)*
