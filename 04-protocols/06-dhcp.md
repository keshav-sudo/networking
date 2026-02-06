# 🏠 DHCP - Dynamic Host Configuration Protocol

---

## 1. Simple Explanation

When you connect to WiFi, how does your device get an IP address automatically? **DHCP!**

DHCP is like an **office receptionist** who assigns desk numbers to visitors:
- You walk in → "Here's desk 42, use it for 8 hours"
- No manual configuration needed

---

## 2. How DHCP Works (DORA Process)

```
Your Device                          DHCP Server
    │                                     │
    │ ── DISCOVER (broadcast) ──────────►│
    │    "Anyone have an IP for me?"      │
    │                                     │
    │ ◄── OFFER ─────────────────────────│
    │    "I can give you 192.168.1.50"   │
    │                                     │
    │ ── REQUEST ───────────────────────►│
    │    "I'll take 192.168.1.50"        │
    │                                     │
    │ ◄── ACK ───────────────────────────│
    │    "Confirmed! It's yours for 24h" │
    │                                     │
Device now has: IP, Subnet Mask, Gateway, DNS servers
```

**DORA = Discover, Offer, Request, Acknowledge**

---

## 3. What DHCP Provides

```
DHCP Response includes:
├── IP Address:      192.168.1.50
├── Subnet Mask:     255.255.255.0
├── Default Gateway: 192.168.1.1 (router)
├── DNS Servers:     8.8.8.8, 8.8.4.4
├── Lease Time:      86400 seconds (24 hours)
└── Domain Name:     home.local
```

---

## 4. DHCP Lease Lifecycle

```
┌─────────────────────────────────────────┐
│          Lease Time: 24 hours          │
├─────────────────────────────────────────┤
│ 0h: Get lease                           │
│ 12h (50%): Try to renew with SAME server│
│ 18h (87.5%): Try ANY DHCP server        │
│ 24h: Lease expires, must get new IP     │
└─────────────────────────────────────────┘

If device leaves network → IP returns to pool
If device stays → Renews same IP usually
```

---

## 5. Where Used?

| Environment | DHCP Use |
|-------------|----------|
| Home WiFi | Router is DHCP server |
| Office | Dedicated DHCP server |
| Cloud (AWS) | VPC DHCP options set |
| Docker | Containers get IPs from bridge |

---

## 6. Practical Commands

```bash
# Linux - Release and renew IP
sudo dhclient -r     # Release
sudo dhclient        # Get new lease

# Mac - Renew DHCP
sudo ipconfig set en0 DHCP

# Windows
ipconfig /release
ipconfig /renew

# View current lease info (Linux)
cat /var/lib/dhcp/dhclient.leases
```

---

## 7. Quick Summary

```
DHCP: Automatic IP assignment

DORA Process:
D - Discover (client broadcasts)
O - Offer (server offers IP)
R - Request (client accepts)
A - Ack (server confirms)

Provides: IP, Mask, Gateway, DNS, Lease time

Static IP: Manually set (servers, printers)
Dynamic IP: DHCP assigned (laptops, phones)
```

---

## 8. Quiz

1. New laptop connects to WiFi. How does it find DHCP server (doesn't know any IPs yet)?
2. Your server should always have same IP. DHCP or static?
3. DHCP lease expires while laptop is asleep. What happens when it wakes?

<details>
<summary>Answers</summary>

1. **Broadcast!** DISCOVER sent to 255.255.255.255 (all devices). Only DHCP server responds with OFFER.

2. **Static IP** for servers (or DHCP reservation). Predictable IP needed for DNS, firewall rules.

3. **New DORA process.** Laptop sends DISCOVER, gets new (or same) IP. May get different IP if original was taken.
</details>

---

*Next: [ARP Protocol](./07-arp.md)*
