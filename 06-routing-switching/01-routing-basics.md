# 🛤 Routing Basics

---

## 1. What is Routing?

**Routing** = Finding the best path for packets to reach destination across networks.

```
Your PC → Router → ISP → ... → Google's network → Google server

Each router decides: "Where do I send this packet next?"
```

---

## 2. Routing Table

```bash
# View routing table
ip route      # Linux
netstat -r    # Mac/Windows
route print   # Windows

Example output:
Destination     Gateway         Interface
0.0.0.0/0       192.168.1.1     eth0    # Default route
192.168.1.0/24  0.0.0.0         eth0    # Local network
10.0.0.0/8      192.168.1.2     eth0    # Via another router
```

---

## 3. How Routing Works

```
Packet destination: 8.8.8.8

Router checks table (most specific first):
1. 8.8.8.8/32?     No match
2. 8.8.8.0/24?     No match
3. 8.8.0.0/16?     No match
4. 0.0.0.0/0?      Match! → Send to gateway

Default route (0.0.0.0/0) catches everything
```

---

## 4. Static vs Dynamic Routing

| Type | Description | Use Case |
|------|-------------|----------|
| **Static** | Manually configured | Small networks |
| **Dynamic** | Routers exchange info | Large networks |

```
Dynamic protocols:
├── RIP (old, simple)
├── OSPF (enterprise)
├── BGP (internet backbone)
└── EIGRP (Cisco)
```

---

## 5. Default Gateway

```
Your device → Default Gateway → Rest of internet

Default Gateway = Your first-hop router
Usually: 192.168.1.1 or 10.0.0.1

Without gateway: Can only talk to local network!
```

---

## 6. Quick Summary

```
Routing: Path selection for packets
Routing Table: Maps destinations to next hops
Default Gateway: Exit point from local network

Static: Manual, simple, doesn't adapt
Dynamic: Automatic, complex, adapts to failures

Most specific match wins!
```

---

*Next: [VLANs](./02-vlans.md)*
