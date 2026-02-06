# 🔍 ARP - Address Resolution Protocol

---

## 1. Simple Explanation

You know the IP address (`192.168.1.5`) but need to send data at **physical layer**. Problem: Network hardware uses **MAC addresses**, not IPs!

**ARP translates IP → MAC address** (like calling reception: "What's John's desk extension?")

---

## 2. How ARP Works

```
Your Computer                           Network
     │                                     │
     │ ── ARP Request (broadcast) ───────►│
     │    "Who has 192.168.1.5?           │
     │     Tell 192.168.1.10"             │
     │                                     │
     │ ◄── ARP Reply (unicast) ───────────│
     │    "I'm 192.168.1.5                │
     │     My MAC is AA:BB:CC:DD:EE:FF"   │
     │                                     │

Your computer caches this in ARP table:
192.168.1.5 → AA:BB:CC:DD:EE:FF
```

---

## 3. ARP Table

```bash
# View ARP cache (your computer)
arp -a

# Output:
# ? (192.168.1.1) at 00:11:22:33:44:55 [ether] on eth0
# ? (192.168.1.5) at AA:BB:CC:DD:EE:FF [ether] on eth0

# Clear ARP cache (Linux)
sudo ip -s -s neigh flush all
```

---

## 4. Why ARP Matters

```
Sending to 192.168.1.5:

Step 1: Do I have MAC for 192.168.1.5?
        Check ARP cache...

Step 2: Not in cache → Send ARP request
        "Who has 192.168.1.5?"

Step 3: Get reply → Update cache
        192.168.1.5 = AA:BB:CC:DD:EE:FF

Step 4: Now can send at Layer 2 (Ethernet)
        Frame TO: AA:BB:CC:DD:EE:FF
```

---

## 5. ARP in Action

| Destination | ARP Needed? |
|-------------|------------|
| Same subnet | Yes - direct ARP for target |
| Different subnet | Yes - ARP for gateway/router |
| Same machine | No (loopback) |

```
Target on different network:
Your PC → ARP for router → Router → Routes to destination
```

---

## 6. ARP Security Issue

```
ARP Spoofing Attack:
──────────────────────
Attacker broadcasts: "I'm 192.168.1.1 (gateway)"
                     "My MAC is ATTACKER-MAC"

Victims update ARP cache
All traffic to gateway → Goes to attacker first!

Defense: Static ARP entries, ARP inspection (switches)
```

---

## 7. Quick Summary

```
ARP: IP → MAC address translation

Works at Layer 2-3 boundary
Broadcast request, unicast reply
Results cached in ARP table

Only for local subnet!
Remote destinations → ARP for gateway only

Related: RARP (MAC → IP, rarely used now)
```

---

## 8. Quiz

1. Same IP, two different MACs in ARP table. Problem?
2. New device joins network. Does it make ARP request first?
3. IPv6 uses ARP? If not, what?

<details>
<summary>Answers</summary>

1. **Yes! Likely ARP spoofing** or duplicate IP. Check for attacks or misconfigured devices.

2. **Usually no.** First needs IP (DHCP). Then when communicating, makes ARP if destination not in cache.

3. **No!** IPv6 uses **NDP (Neighbor Discovery Protocol)** with ICMPv6. Similar purpose, better design.
</details>

---

*Next: [FTP, SFTP, SSH](./08-ftp-sftp-ssh.md)*
