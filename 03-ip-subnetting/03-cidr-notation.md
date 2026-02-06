# 🔢 CIDR Notation & Subnetting

---

## 1. Simple Explanation (Beginner Level)

**CIDR = Classless Inter-Domain Routing**

Instead of saying "my network uses 255.255.255.0 subnet mask", we write `/24`.

```
192.168.1.0/24
      │     │
      │     └── 24 bits for network, 8 bits for hosts
      └── Network address
```

The number after `/` = how many bits are for the network.

---

## 2. Real-World Analogy

```
/24 = Apartment Building
      192.168.1.x
      256 apartments (0-255) on one address

/16 = Whole Neighborhood
      192.168.x.x  
      65,536 addresses

/8 = Entire City
     10.x.x.x
     16 million addresses
```

---

## 3. Technical Working

### CIDR to Subnet Mask Conversion

| CIDR | Subnet Mask | Addresses | Usable Hosts |
|------|-------------|-----------|--------------|
| /8 | 255.0.0.0 | 16,777,216 | 16,777,214 |
| /16 | 255.255.0.0 | 65,536 | 65,534 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /30 | 255.255.255.252 | 4 | 2 |
| /32 | 255.255.255.255 | 1 | 1 (single host) |

**Formula:** Usable = 2^(32-CIDR) - 2 (subtract network & broadcast)

### Subnet Calculation Example

```
Given: 192.168.10.0/26

Step 1: /26 means 26 bits for network, 6 bits for hosts
Step 2: 2^6 = 64 addresses per subnet
Step 3: Usable hosts = 64 - 2 = 62

Network:   192.168.10.0
First host: 192.168.10.1
Last host:  192.168.10.62
Broadcast:  192.168.10.63

Next subnet: 192.168.10.64/26
```

### Subnetting Visual

```
192.168.1.0/24 split into 4 /26 subnets:

│ 192.168.1.0/26   │ 192.168.1.64/26  │
│ Hosts: 1-62      │ Hosts: 65-126    │
├──────────────────┼──────────────────┤
│ 192.168.1.128/26 │ 192.168.1.192/26 │
│ Hosts: 129-190   │ Hosts: 193-254   │
```

---

## 4. Where Used?

| Context | Example |
|---------|---------|
| AWS VPC | 10.0.0.0/16 (VPC), 10.0.1.0/24 (subnet) |
| Docker | 172.17.0.0/16 default bridge |
| Kubernetes | /16 for pod network |
| Firewall rules | Allow 10.0.0.0/8 |

---

## 5. Practical Example

### AWS VPC Design

```
VPC: 10.0.0.0/16 (65,536 IPs)
├── Public Subnet:  10.0.1.0/24 (256 IPs)
├── Private Subnet: 10.0.2.0/24 (256 IPs)
├── Database Subnet: 10.0.3.0/24 (256 IPs)
└── Spare: 10.0.4.0/22 (1024 IPs for future)
```

### Quick CIDR Calculator

```bash
# Using ipcalc
ipcalc 192.168.1.0/26

# Output:
# Network:   192.168.1.0/26
# Netmask:   255.255.255.192
# HostMin:   192.168.1.1
# HostMax:   192.168.1.62
# Broadcast: 192.168.1.63
# Hosts/Net: 62
```

---

## 6. Quick Summary

```
CIDR: /XX notation replaces subnet masks

Common:
/24 = 256 IPs = 254 usable
/16 = 65,536 IPs
/32 = 1 IP (single host)

Calculate hosts: 2^(32-CIDR) - 2

Cloud design: Start with /16, use /24 subnets
```

---

## 7. Quiz

1. How many usable hosts in /28?
2. 10.0.0.0/8 - is 10.255.255.255 inside this range?
3. You need 100 hosts. Minimum CIDR?

<details>
<summary>Answers</summary>

1. **/28 = 2^4 - 2 = 14 usable hosts**

2. **No!** 10.255.255.255 is the broadcast address for 10.0.0.0/8. It's in the range but not usable.

3. **/25 (128-2=126 hosts).** /26 only gives 62, too small.
</details>

---

*Next: [Subnetting Practice](./04-subnetting-practice.md)*
