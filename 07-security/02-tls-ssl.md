# 🔒 TLS/SSL Deep Dive

---

## 1. Simple Explanation (Beginner Level)

**TLS (Transport Layer Security)** encrypts data between two computers so nobody in between can read it.

- **SSL**: Old name (versions 1-3, deprecated)
- **TLS**: Current name (versions 1.0-1.3, use 1.2 or 1.3)

When you see HTTPS, the S means TLS is protecting your HTTP traffic.

```
Without TLS:  You → "password123" →→ Attacker can see →→ Server
With TLS:    You → "a7#kL9..." (encrypted) →→ Can't read →→ Server
```

---

## 2. Real-World Analogy

```
TLS = Diplomatic Pouch

Without TLS:
📝 Letter (readable by anyone)
→ Delivery person (can read it)
→ Recipient

With TLS:
📝 Letter → 🔒 Locked diplomatic pouch (only recipient has key)
→ Delivery person (sees pouch, can't read letter)
→ Recipient → 🔑 Unlocks
```

---

## 3. Technical Working

### TLS 1.2 Handshake

```
Client                                  Server
   │                                       │
   │ ─── ClientHello ───────────────────►  │
   │     TLS version, cipher suites,       │
   │     random number                     │
   │                                       │
   │ ◄─── ServerHello ───────────────────  │
   │     Chosen cipher, random number      │
   │                                       │
   │ ◄─── Certificate ───────────────────  │
   │     Server's public certificate       │
   │                                       │
   │ [Client verifies certificate]         │
   │   - Signed by trusted CA?             │
   │   - Domain name matches?              │
   │   - Not expired?                      │
   │                                       │
   │ ─── ClientKeyExchange ─────────────►  │
   │     Pre-master secret (encrypted)     │
   │                                       │
   │ ─── ChangeCipherSpec ──────────────►  │
   │ ─── Finished ──────────────────────►  │
   │                                       │
   │ ◄─── ChangeCipherSpec ──────────────  │
   │ ◄─── Finished ──────────────────────  │
   │                                       │
   │ ═══ Encrypted Application Data ═══    │
```

### What TLS Provides

| Property | What It Means |
|----------|--------------|
| **Encryption** | Data unreadable to eavesdroppers |
| **Authentication** | Server is who it claims to be |
| **Integrity** | Data hasn't been modified |

### Certificates Chain

```
Root CA (Self-signed, pre-installed in OS/browser)
    │
    └── Intermediate CA (Signed by Root)
            │
            └── Your Certificate (Signed by Intermediate)

Browser verifies chain: Your Cert → Intermediate → Root ✓
```

---

## 4. Where Used in Real Systems?

- **HTTPS**: HTTP over TLS
- **Database connections**: MySQL/PostgreSQL with SSL
- **Email**: SMTP/IMAP with TLS
- **Internal services**: Microservices communication

---

## 5. Practical Example

### Generate Self-Signed Certificate

```bash
# Generate private key and certificate
openssl req -x509 -newkey rsa:4096 \
  -keyout key.pem -out cert.pem \
  -days 365 -nodes \
  -subj "/CN=localhost"

# View certificate details
openssl x509 -in cert.pem -text -noout
```

### HTTPS Server (Node.js)

```javascript
const https = require('https');
const fs = require('fs');

const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};

https.createServer(options, (req, res) => {
  res.writeHead(200);
  res.end('Secure!');
}).listen(443);
```

### Check Website Certificate

```bash
# View certificate
openssl s_client -connect google.com:443 -showcerts

# Check expiration
echo | openssl s_client -connect google.com:443 2>/dev/null | \
  openssl x509 -noout -dates
```

---

## 6. Common Mistakes

| ❌ Mistake | ✅ Correct |
|-----------|-----------|
| "SSL and TLS are same" | SSL is deprecated; always say TLS |
| "Self-signed is OK for production" | Browsers won't trust it; use Let's Encrypt |
| "TLS encrypts domain" | Domain visible in SNI during handshake |

---

## 7. Quick Summary

```
TLS: Encrypts communication between client and server

Provides: Encryption, Authentication, Integrity

Handshake:
1. ClientHello → ServerHello
2. Certificate exchange
3. Key exchange (create shared secret)
4. Encrypted communication

Use TLS 1.2 or 1.3 (older versions deprecated)

Certificates:
- Get from CA (Let's Encrypt is free)
- Chain: Root → Intermediate → Your cert
```

---

## 8. Quiz Questions

1. Why is TLS 1.0 considered insecure?
2. Your certificate expires. What happens to users?
3. Can you have HTTPS without a signed certificate?
4. What's SNI and why does it matter for privacy?
5. TLS connection is slow first time, fast after. Why?

---

## 9. Answer Key

<details>
<summary>Click to reveal</summary>

1. **Vulnerable to POODLE, BEAST attacks**. Weak ciphers, protocol flaws. Use TLS 1.2+.

2. **Certificate errors!** Browsers show scary warning. Users can't access site securely. Renew before expiration!

3. **Yes, but browsers warn.** Self-signed works technically but shows "Not Secure" warning.

4. **Server Name Indication**: Domain sent in plain text during handshake (before encryption). Privacy concern - ISP/attacker sees what domain you're visiting. ESNI aims to fix this.

5. **Session resumption!** After first handshake, session key cached. Subsequent connections skip full handshake.

</details>

---

*Next: [Certificates & PKI](./03-certificates-pki.md)*
