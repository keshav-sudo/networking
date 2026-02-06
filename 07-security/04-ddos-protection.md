# 🛡 DDoS Protection

---

## 1. What is DDoS?

**Distributed Denial of Service** - Overwhelm your server with traffic from many sources.

```
Normal: 1000 req/sec → Server handles fine
DDoS:   1,000,000 req/sec → Server crashes

Attackers use botnets (thousands of infected devices)
```

---

## 2. Attack Types

```
Volume-based:
├── UDP Flood
├── ICMP Flood
└── Goal: Saturate bandwidth

Protocol-based:
├── SYN Flood (half-open connections)
├── Ping of Death
└── Goal: Exhaust server resources

Application-based:
├── HTTP Flood
├── Slowloris
└── Goal: Crash application layer
```

---

## 3. Protection Strategies

```
1. CDN/WAF (Cloudflare, AWS Shield)
   └── Absorbs attack traffic at edge

2. Rate Limiting
   └── Block IPs with too many requests

3. Anycast
   └── Distribute traffic across multiple locations

4. Traffic Filtering
   └── Drop malicious packets early

5. Auto-scaling
   └── Temporarily scale up to handle load
```

---

## 4. Implementation

```javascript
// Rate limiting in Express
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,  // 15 minutes
  max: 100,  // 100 requests per window
  message: 'Too many requests'
});

app.use(limiter);
```

```nginx
# Nginx rate limiting
limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;

server {
    location / {
        limit_req zone=one burst=20;
    }
}
```

---

## 5. Quick Summary

```
DDoS: Overwhelm with traffic from many sources

Types:
├── Volume: Bandwidth saturation
├── Protocol: Exhaust resources
└── Application: Target app layer

Protection:
├── CDN/WAF: Cloudflare, AWS Shield
├── Rate limiting
├── Traffic filtering
└── Auto-scaling

You can't prevent DDoS, only absorb/mitigate it
```

---

*Security module complete!*
