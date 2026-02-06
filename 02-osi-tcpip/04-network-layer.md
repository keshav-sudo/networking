# 🌐 Network Layer (Layer 3)

---

## 1. What It Does

Network Layer handles **IP addressing** and **routing** - getting packets across different networks to reach destination.

Think: How does data travel from your house to Google's servers?

---

## 2. Key Concepts

```
IP Address: Logical address (can change)
Example: 192.168.1.10

Packet = [ IP Header | Data ]
         Source IP, Dest IP, TTL

Routing: Determining path across networks
```

---

## 3. IP Header

```
┌────────────────────────────────────────────────┐
│ Version │ Header Len │ ToS │ Total Length      │
├────────────────────────────────────────────────┤
│ Identification   │ Flags │ Fragment Offset    │
├────────────────────────────────────────────────┤
│ TTL │ Protocol │ Header Checksum              │
├────────────────────────────────────────────────┤
│              Source IP Address                 │
├────────────────────────────────────────────────┤
│           Destination IP Address               │
├────────────────────────────────────────────────┤
│               Data...                          │
└────────────────────────────────────────────────┘

TTL: Time To Live (prevents infinite loops)
Protocol: TCP (6), UDP (17), ICMP (1)
```

---

## 4. Devices at Layer 3

| Device | Function |
|--------|----------|
| **Router** | Forwards packets between networks |
| **Layer 3 Switch** | Switch with routing capability |

---

## 5. Routing

```
Packet destination: 8.8.8.8

Router checks routing table:
├── 192.168.1.0/24 → Local (eth0)
├── 10.0.0.0/8     → via 192.168.1.2
└── 0.0.0.0/0      → via 192.168.1.1 (default gateway)

8.8.8.8 matches default → Send to gateway
```

---

## 6. Quick Summary

```
Network Layer = IP addressing + Routing

Addressing: IP addresses (logical)
Devices: Routers, L3 switches
Key protocols: IP, ICMP (ping), OSPF, BGP

Packets routed between networks
TTL prevents infinite loops
```

---

*Next: [Transport Layer](./05-transport-layer.md)*
