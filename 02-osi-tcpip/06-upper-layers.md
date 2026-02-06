# 🎭 Session, Presentation, Application Layers (5, 6, 7)

---

## Layer 5: Session Layer

**Manages sessions (connections) between applications**

```
Functions:
├── Establish connection
├── Maintain session
├── Terminate cleanly
└── Synchronization (checkpoints)

Examples: NetBIOS, RPC, login sessions
```

---

## Layer 6: Presentation Layer

**Data translation, encryption, compression**

```
Functions:
├── Data format conversion (EBCDIC → ASCII)
├── Encryption/Decryption (SSL/TLS)
├── Compression
└── Character encoding (UTF-8)

Examples: SSL/TLS, JPEG, MPEG, encryption
```

---

## Layer 7: Application Layer

**User-facing protocols and applications**

```
Functions:
├── Network services to applications
├── User interface to network
└── Protocol for specific applications

Examples: HTTP, FTP, SMTP, DNS, SSH
```

---

## In Practice (TCP/IP Model)

```
OSI:                 TCP/IP:
┌─────────────────┐
│ 7. Application  │
├─────────────────┤  ┌─────────────────┐
│ 6. Presentation │──│   Application   │
├─────────────────┤  └─────────────────┘
│ 5. Session      │
└─────────────────┘

In real world, these 3 layers are combined!
HTTP does application + some presentation work
TLS handles encryption (presentation)
TCP handles session management
```

---

## Quick Summary

```
Layer 5 (Session): Start/stop connections
Layer 6 (Presentation): Encrypt, compress, format
Layer 7 (Application): HTTP, DNS, actual apps

In practice: Usually combined into "Application layer"
Most protocols (HTTP, FTP) span all three
```

---

*Next: [TCP/IP Model](./07-tcpip-model.md)*
