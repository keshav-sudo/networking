# 🎭 Subnet Masks

---

## 1. What is a Subnet Mask?

Subnet mask tells which part of IP is **network** and which is **host**.

```
IP:          192.168.1.100
Subnet Mask: 255.255.255.0

Network:     192.168.1.x    (where mask is 255)
Host:        x.x.x.100      (where mask is 0)
```

---

## 2. How It Works (AND Operation)

```
IP:        192.168.1.100  = 11000000.10101000.00000001.01100100
Mask:      255.255.255.0  = 11111111.11111111.11111111.00000000
                            ─────────────────────────────────────
Network:   192.168.1.0    = 11000000.10101000.00000001.00000000

Bitwise AND: 1 & 1 = 1, anything else = 0
```

---

## 3. Common Subnet Masks

| Mask | CIDR | Binary (last octet) | Hosts |
|------|------|---------------------|-------|
| 255.255.255.0 | /24 | 00000000 | 254 |
| 255.255.255.128 | /25 | 10000000 | 126 |
| 255.255.255.192 | /26 | 11000000 | 62 |
| 255.255.255.224 | /27 | 11100000 | 30 |
| 255.255.255.240 | /28 | 11110000 | 14 |
| 255.255.255.252 | /30 | 11111100 | 2 |

---

## 4. Same Network Check

```
Device A: 192.168.1.10 / 255.255.255.0
Device B: 192.168.1.50 / 255.255.255.0

Apply mask to both:
A: 192.168.1.0
B: 192.168.1.0

Same network! → Can communicate directly

Device C: 192.168.2.10 / 255.255.255.0
Apply mask: 192.168.2.0

Different network → Need router
```

---

## 5. Quick Summary

```
Subnet Mask: Divides IP into network + host

255 = network bits (fixed)
0 = host bits (variable)

/24 = 255.255.255.0 = Most common
/16 = 255.255.0.0 = Large networks
/32 = Single host

Same network = Same result after masking
```

---

*Next: [CIDR Notation](./03-cidr-notation.md)*
