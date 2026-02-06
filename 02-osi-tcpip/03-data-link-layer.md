# 📡 Data Link Layer (Layer 2)

---

## 1. What It Does

Data Link Layer packages bits into **frames** and handles **MAC addresses** for local network communication.

Think: How do devices on the SAME network talk?

---

## 2. Key Concepts

```
MAC Address: 48-bit hardware address
Example: AA:BB:CC:DD:EE:FF

Frame = [ Header | Data | Trailer ]
        MAC addresses, error checking
```

---

## 3. Sub-Layers

```
┌─────────────────────────────────┐
│  LLC (Logical Link Control)     │
│  - Error handling               │
│  - Flow control                 │
├─────────────────────────────────┤
│  MAC (Media Access Control)      │
│  - MAC addressing               │
│  - Frame transmission           │
└─────────────────────────────────┘
```

---

## 4. Devices at Layer 2

| Device | Function |
|--------|----------|
| **Switch** | Forwards frames based on MAC address |
| **Bridge** | Connects network segments |
| **NIC** | Has the MAC address |

---

## 5. Ethernet Frame

```
┌──────────┬──────────┬──────┬──────┬─────┬─────┐
│ Preamble │ Dest MAC │ Src  │ Type │Data │ FCS │
│   8B     │   6B     │ MAC  │  2B  │     │ 4B  │
│          │          │  6B  │      │     │     │
└──────────┴──────────┴──────┴──────┴─────┴─────┘

FCS = Frame Check Sequence (error detection)
```

---

## 6. Quick Summary

```
Data Link = Frame-based local communication

Addressing: MAC addresses
Devices: Switches, bridges
Error detection: CRC/FCS

ARP works here (IP → MAC)
VLANs configured here
```

---

*Next: [Network Layer](./04-network-layer.md)*
