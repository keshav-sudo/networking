# 🏷 VLANs - Virtual LANs

---

## 1. What is VLAN?

**VLAN** = Logically separate networks on same physical switch.

```
Without VLAN:
All ports on switch = One broadcast domain
Everyone hears everyone's broadcast

With VLAN:
Port 1-10: VLAN 10 (Sales)
Port 11-20: VLAN 20 (Engineering)
Sales can't see Engineering traffic!
```

---

## 2. Why VLANs?

```
Benefits:
├── Security: Isolate departments
├── Performance: Reduce broadcast traffic  
├── Flexibility: Move users without rewiring
└── Organization: Logical grouping
```

---

## 3. VLAN Types

```
Access Port: One VLAN only
└── Your laptop connects here
└── Untagged traffic

Trunk Port: Multiple VLANs
└── Between switches
└── Tagged traffic (802.1Q)
```

---

## 4. 802.1Q Tagging

```
Normal Frame:
[Dest MAC][Src MAC][Type][Data][FCS]

Tagged Frame (VLAN):
[Dest MAC][Src MAC][802.1Q Tag][Type][Data][FCS]
                    └── VLAN ID (12 bits = 4096 VLANs)
```

---

## 5. Inter-VLAN Routing

```
VLAN 10 needs to talk to VLAN 20?
Need a router or Layer 3 switch!

               ┌──────────┐
VLAN 10 ───────│  Router  │─────── VLAN 20
               └──────────┘
```

---

## 6. Quick Summary

```
VLAN: Logical network segmentation
Access port: Single VLAN (endpoints)
Trunk port: Multiple VLANs (between switches)

Tags: 802.1Q adds VLAN ID to frames
Inter-VLAN: Needs router or L3 switch

Use for: Security, organization, performance
```

---

*Next: [Switches](./03-switches.md)*
