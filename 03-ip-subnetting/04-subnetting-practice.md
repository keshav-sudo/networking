# ✏️ Subnetting Practice Problems

---

## Problem 1: Basic Subnet

**Given:** 192.168.10.0/24, need 4 equal subnets

```
Solution:
/24 = 256 IPs total
4 subnets = 256/4 = 64 IPs each = /26

Subnet 1: 192.168.10.0/26   (0-63)
Subnet 2: 192.168.10.64/26  (64-127)
Subnet 3: 192.168.10.128/26 (128-191)
Subnet 4: 192.168.10.192/26 (192-255)

Each subnet: 62 usable hosts
```

---

## Problem 2: Host Requirement

**Given:** Need subnet for 50 hosts

```
Solution:
50 hosts + network + broadcast = 52
2^n >= 52 → n = 6 (2^6 = 64)
32 - 6 = /26

Use /26 = 62 usable hosts ✓
```

---

## Problem 3: Find Network Address

**Given:** Host 172.16.45.100/20

```
Solution:
/20 = 255.255.240.0

172.16.45.100 AND 255.255.240.0

45 in binary: 00101101
240 in binary: 11110000
45 AND 240 = 00100000 = 32

Network: 172.16.32.0/20
```

---

## Problem 4: AWS VPC Design

**Given:** VPC 10.0.0.0/16, need:
- 2 public subnets (250 hosts each)
- 2 private subnets (1000 hosts each)
- 2 database subnets (50 hosts each)

```
Solution:
Public:   /24 gives 254 hosts → 10.0.1.0/24, 10.0.2.0/24
Private:  /22 gives 1022 hosts → 10.0.4.0/22, 10.0.8.0/22
Database: /26 gives 62 hosts → 10.0.16.0/26, 10.0.16.64/26
```

---

## Quick Reference

```
Hosts needed → CIDR
2-6:    /29 (6 hosts)
7-14:   /28 (14 hosts)
15-30:  /27 (30 hosts)
31-62:  /26 (62 hosts)
63-126: /25 (126 hosts)
127-254: /24 (254 hosts)
```

---

*Practice these! Subnetting is common in interviews.*
