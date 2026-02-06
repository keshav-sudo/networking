# 🌐 Routers

---

## 1. What is a Router?

A **router** connects different networks and forwards packets based on **IP addresses**.

```
Switch: Local network (Layer 2, MAC)
Router: Between networks (Layer 3, IP)

Your home: PC → Switch → Router → Internet
```

---

## 2. Router Functions

```
1. Routing: Determine best path
2. Forwarding: Send packet to next hop
3. NAT: Translate private to public IP
4. DHCP: Assign IP addresses
5. Firewall: Filter traffic
```

---

## 3. Router vs Switch

| Feature | Switch | Router |
|---------|--------|--------|
| Layer | 2 (Data Link) | 3 (Network) |
| Address | MAC | IP |
| Domain | Broadcast | Separate |
| Speed | Faster | Slower |
| Scope | Local network | Between networks |

---

## 4. Routing Decision

```
Packet arrives for 8.8.8.8

Router checks:
1. Is 8.8.8.8 directly connected? No
2. Do I have specific route? No
3. Use default route → Send to ISP

Next router repeats until destination reached
```

---

## 5. Quick Summary

```
Router: Layer 3 device, uses IP addresses
Connects different networks
Makes routing decisions based on routing table

Functions: Route, NAT, DHCP, Firewall
Each interface = different network/subnet

Home router: Router + Switch + WiFi + Firewall combined
```

---

*Next: [NAT](./05-nat.md)*
