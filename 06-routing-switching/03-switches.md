# 🔀 Switches

---

## 1. What is a Switch?

A **switch** connects devices on a local network and forwards frames based on **MAC addresses**.

```
Hub (old): Sends to ALL ports
Switch: Sends only to correct port

Much more efficient!
```

---

## 2. How Switches Learn

```
1. Frame arrives on port 1 from MAC AA:AA:AA
2. Switch records: "AA:AA:AA is on port 1"
3. Frame destined for BB:BB:BB
4. Check table... BB:BB:BB on port 3
5. Send frame only to port 3

MAC Address Table:
┌────────────────┬──────┐
│ MAC            │ Port │
├────────────────┼──────┤
│ AA:AA:AA:AA:AA │ 1    │
│ BB:BB:BB:BB:BB │ 3    │
│ CC:CC:CC:CC:CC │ 5    │
└────────────────┴──────┘
```

---

## 3. Switch Types

| Type | Layer | Capability |
|------|-------|------------|
| **Unmanaged** | 2 | Plug and play, no config |
| **Managed** | 2 | VLANs, monitoring, config |
| **Layer 3** | 2+3 | Can route between VLANs |

---

## 4. Key Concepts

```
Collision Domain: Each port is separate (full duplex)
Broadcast Domain: All ports same (unless VLANs)
Spanning Tree: Prevents loops
Port Mirroring: Copy traffic for monitoring
```

---

## 5. Quick Summary

```
Switch: Layer 2 device, uses MAC addresses
Learns MAC-to-port mapping automatically
More efficient than hub

Managed switches: VLANs, monitoring, config
Layer 3 switches: Can also route (Layer 3)
```

---

*Next: [Routers](./04-routers.md)*
