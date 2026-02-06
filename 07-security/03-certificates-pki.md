# 🎫 Certificates & PKI

---

## 1. What is a Certificate?

A **digital certificate** proves identity - like a passport for servers.

```
Certificate says:
├── This is google.com
├── Public key is: [abc123...]
├── Verified by: DigiCert (trusted CA)
├── Valid until: Dec 2025
└── Signature: [CA's signature]
```

---

## 2. How It Works

```
1. Browser connects to https://google.com
2. Server sends its certificate
3. Browser checks:
   ├── Is it signed by trusted CA?
   ├── Is domain matching?
   ├── Is it expired?
   └── Is it revoked?
4. If OK → Encrypted communication
5. If not → Warning "Not Secure"
```

---

## 3. Certificate Chain

```
Root CA (pre-installed in browser/OS)
    │
    └── Intermediate CA (signed by Root)
            │
            └── Your Certificate (signed by Intermediate)

Why chain?
Root stays offline (super secure)
Intermediate does daily signing
```

---

## 4. Getting a Certificate

```bash
# Option 1: Free - Let's Encrypt
sudo certbot --nginx -d yourdomain.com

# Option 2: Self-signed (for testing)
openssl req -x509 -newkey rsa:4096 \
  -keyout key.pem -out cert.pem \
  -days 365 -nodes -subj "/CN=localhost"

# Option 3: Paid CAs (DigiCert, Comodo)
└── For EV (Extended Validation) green bar
```

---

## 5. Certificate Types

| Type | Validation | Use Case |
|------|-----------|----------|
| **DV** | Domain only | Basic websites |
| **OV** | Organization verified | Business sites |
| **EV** | Extended validation | Banks, e-commerce |

---

## 6. Quick Summary

```
Certificate: Digital identity proof
CA: Certificate Authority (trusted issuer)
PKI: Public Key Infrastructure (the whole system)

Chain: Root → Intermediate → Your cert
Let's Encrypt: Free automated certs

Renewal: Set up auto-renewal (certs expire!)
```

---

*Next: [DDoS Protection](./04-ddos-protection.md)*
