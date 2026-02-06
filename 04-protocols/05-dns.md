# 🌐 DNS - Domain Name System

---

## 1. Simple Explanation (Beginner Level)

**DNS = The Phonebook of the Internet**

When you type `google.com` in your browser, your computer doesn't understand "google.com". Computers only understand numbers (IP addresses like `142.250.185.78`).

**DNS is the system that converts names to numbers.**

```
You type: google.com
DNS says: That's IP 142.250.185.78
Browser: *connects to 142.250.185.78*
```

Without DNS, you would need to remember IP addresses for every website! Imagine typing `142.250.185.78` instead of `google.com`.

---

## 2. Real-World Analogy

### 📱 Phone Contacts Analogy

| Without Contacts | With Contacts |
|-----------------|---------------|
| Memorize: +91-9876543210 | Just tap "Mom" |
| Memorize: +1-555-123-4567 | Just tap "John" |

DNS is like your phone's contact list for the entire internet!

### 🏢 Office Directory Analogy

```
Company Reception (DNS Server):

You: "I want to meet John from Engineering"

Reception: "Let me check the directory..."
           "John is in Building B, Room 304"

You: *goes to Building B, Room 304*

Without reception: You'd need to know everyone's exact location!
```

---

## 3. Technical Working (Step-by-Step)

### DNS Hierarchy

```
                    ┌─────────────────┐
                    │  Root DNS (.)   │  ← 13 root server clusters
                    │  "I know TLDs"  │     (managed globally)
                    └────────┬────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
   ┌─────┴─────┐      ┌─────┴─────┐      ┌─────┴─────┐
   │ .com TLD  │      │ .org TLD  │      │ .io TLD   │
   │           │      │           │      │           │
   └─────┬─────┘      └───────────┘      └───────────┘
         │
   ┌─────┴─────┐
   │ google.com│  ← Authoritative NS for google.com
   │   NS      │     (Google controls this)
   └─────┬─────┘
         │
   ┌─────┴─────────────┐
   │ www.google.com    │
   │ mail.google.com   │
   │ maps.google.com   │
   └───────────────────┘
```

### DNS Record Types

| Type | Purpose | Example |
|------|---------|---------|
| **A** | Domain → IPv4 | google.com → 142.250.185.78 |
| **AAAA** | Domain → IPv6 | google.com → 2607:f8b0:4004:800::200e |
| **CNAME** | Alias | www.google.com → google.com |
| **MX** | Mail server | google.com → smtp.google.com |
| **TXT** | Text data | Used for verification, SPF |
| **NS** | Name server | google.com NS → ns1.google.com |
| **PTR** | Reverse lookup | IP → Domain |
| **SOA** | Zone authority | Primary NS, admin email |

### DNS Resolution Process (Step by Step)

```
User types: www.example.com

Step 1: Check Browser Cache
   └── Found? → Use it! (Skip remaining steps)
   └── Not found? → Continue...

Step 2: Check OS Cache
   └── Mac: scutil --dns
   └── Linux: systemd-resolve --statistics

Step 3: Query Resolver (usually your ISP or 8.8.8.8)
   ┌────────────────────────────────────────────────────────┐
   │  Your Computer → Recursive Resolver (e.g., 8.8.8.8)    │
   │       "What's the IP for www.example.com?"             │
   └────────────────────────────────────────────────────────┘

Step 4: Resolver queries Root DNS
   └── "Who handles .com?"
   └── Root: "Ask .com TLD servers at x.x.x.x"

Step 5: Resolver queries .com TLD
   └── "Who handles example.com?"
   └── TLD: "Ask example.com's NS at y.y.y.y"

Step 6: Resolver queries example.com's Authoritative NS
   └── "What's the IP for www.example.com?"
   └── NS: "It's 93.184.216.34"

Step 7: Resolver returns answer to you
   └── Your computer caches it (TTL based)
   └── Browser connects to 93.184.216.34
```

### Visual Flow:

```
Your Browser                                    
     │                                          
     │ "www.example.com?"                      
     ↓                                          
┌────────────┐                                  
│ Local DNS  │  ← Cache check                   
│  Resolver  │                                  
└─────┬──────┘                                  
      │ Not in cache                            
      ↓                                          
┌────────────┐      ┌──────────────┐             
│ Recursive  │─────→│  Root DNS    │             
│ Resolver   │      │  (.)         │             
│ (8.8.8.8)  │←─────│ ".com at X"  │             
└─────┬──────┘      └──────────────┘             
      │                                          
      ↓                                          
┌────────────┐      ┌──────────────┐             
│ Recursive  │─────→│  .com TLD    │             
│ Resolver   │      │              │             
│            │←─────│"example NS Y"│             
└─────┬──────┘      └──────────────┘             
      │                                          
      ↓                                          
┌────────────┐      ┌──────────────┐             
│ Recursive  │─────→│ example.com  │             
│ Resolver   │      │ Authoritative│             
│            │←─────│"93.184.216.34"│             
└─────┬──────┘      └──────────────┘             
      │                                          
      ↓                                          
   Your Browser: "Connect to 93.184.216.34"     
```

---

## 4. Where is This Used in Real Systems?

### Backend Development:

| Scenario | DNS Usage |
|----------|-----------|
| **API domains** | api.yourapp.com → Load balancer IP |
| **Microservices** | Internal DNS like service-a.internal → Service IP |
| **Database connections** | db.yourapp.com → Database server IP |
| **CDN** | Based on location, different IPs |
| **Email** | MX records for mail delivery |
| **Service discovery** | Kubernetes uses DNS for pod discovery |

### DNS in Cloud Architecture:

```
Internet Users
      │
      │  DNS Query: api.myapp.com
      ↓
┌─────────────────┐
│   Route 53      │  ← AWS DNS Service
│   (DNS)         │
└────────┬───────┘
         │ Returns: Load Balancer IP
         ↓
┌─────────────────┐
│ Load Balancer   │
│ 52.1.2.3        │
└────────┬───────┘
         │ Routes to healthy servers
    ┌────┴────┐
    ↓         ↓
┌────────┐ ┌────────┐
│Server 1│ │Server 2│
└────────┘ └────────┘
```

### DNS-Based Load Balancing:

```
Round-Robin DNS:
api.myapp.com → 10.0.0.1  (33% traffic)
api.myapp.com → 10.0.0.2  (33% traffic)
api.myapp.com → 10.0.0.3  (33% traffic)

Geo DNS (Route 53):
User in India      → api.myapp.com → Mumbai server
User in USA        → api.myapp.com → Virginia server
User in Europe     → api.myapp.com → Frankfurt server
```

---

## 5. Practical Example (Web/Backend)

### Example 1: DNS Lookup Commands

```bash
# Basic lookup
nslookup google.com
# Server:  8.8.8.8
# Address: 142.250.185.78

# Detailed lookup with dig
dig google.com

# Specific record types
dig google.com A        # IPv4
dig google.com AAAA     # IPv6
dig google.com MX       # Mail servers
dig google.com NS       # Name servers
dig google.com TXT      # Text records

# Trace full resolution path
dig +trace google.com

# Check specific DNS server
dig @8.8.8.8 google.com
dig @1.1.1.1 google.com
```

### Example 2: DNS Lookup in Node.js

```javascript
const dns = require('dns');
const { promisify } = require('util');

const resolve4 = promisify(dns.resolve4);
const resolveMx = promisify(dns.resolveMx);

async function lookupDomain(domain) {
  try {
    // Get IPv4 addresses
    const addresses = await resolve4(domain);
    console.log(`${domain} IPs:`, addresses);
    
    // Get mail servers
    const mxRecords = await resolveMx(domain);
    console.log(`${domain} Mail Servers:`, mxRecords);
    
  } catch (err) {
    console.error('DNS Error:', err.message);
  }
}

lookupDomain('google.com');

// Output:
// google.com IPs: [ '142.250.185.78' ]
// google.com Mail Servers: [ { exchange: 'smtp.google.com', priority: 10 } ]
```

### Example 3: Setting Up DNS Records

```bash
# If you're using CloudFlare, Route 53, or any DNS provider:

# A Record (Domain to IP)
example.com     A     192.168.1.100

# CNAME (Alias)
www.example.com CNAME example.com
api.example.com CNAME lb.aws.amazon.com

# MX Record (Email)
example.com     MX    10 mail.example.com
example.com     MX    20 backup-mail.example.com

# TXT Record (Verification)
example.com     TXT   "v=spf1 include:_spf.google.com ~all"
```

### Example 4: DNS Caching Issue (Common Bug)

```javascript
// Problem: DNS cached, but server IP changed!
// Old IP still being used

// Solution in Node.js - disable DNS caching
const http = require('http');

// Option 1: Set DNS cache TTL to 0
const dns = require('dns');
dns.setDefaultResultOrder('ipv4first');

// Option 2: Use custom DNS lookup
const agent = new http.Agent({
  // Don't cache DNS results
  lookup: (hostname, options, callback) => {
    dns.lookup(hostname, options, callback);
  }
});

// Why this matters:
// - Blue-green deployments: IP changes
// - Auto-scaling: New IPs added
// - Failover: Traffic shifted to backup
```

### Example 5: Local DNS for Development

```bash
# Edit hosts file (bypasses DNS)
# Mac/Linux: /etc/hosts
# Windows: C:\Windows\System32\drivers\etc\hosts

# Add entries:
127.0.0.1   myapp.local
127.0.0.1   api.myapp.local
192.168.1.100   db.myapp.local

# Now:
# ping myapp.local → 127.0.0.1
# No DNS query made!
```

---

## 6. Common Mistakes & Misunderstandings

| ❌ Mistake | ✅ Correct Understanding |
|-----------|-------------------------|
| "DNS changes are instant" | DNS has TTL (Time To Live). Changes can take minutes to days to propagate |
| "DNS is always fast" | First lookup can be slow; subsequent are cached |
| "Every website has unique IP" | Multiple domains can share IP (virtual hosting) |
| "DNS is simple lookup" | DNS involves multiple servers, caching, and can be complex |
| "Changing DNS = Changing IP" | DNS just points to IP; changing DNS record still requires the new IP to work |

### Interview-Focused Insight:

> **Q: What is DNS caching and why can it cause issues?**
>
> **A:** DNS responses are cached at multiple levels:
> 1. **Browser cache** (~minutes)
> 2. **OS cache** (~hours)
> 3. **Resolver cache** (ISP/8.8.8.8) (~TTL)
>
> **Issues:**
> - Server IP change? Old IP still cached!
> - Deploying to new servers? Some users see old servers
> - Failover? Takes time for traffic to shift
>
> **Solutions:**
> - Lower TTL before changes
> - Use short TTL for dynamic systems
> - Blue-green: Keep old servers running during transition

---

## 7. Quick Summary (Revision Notes)

```
┌──────────────────────────────────────────────────────────────────┐
│                       DNS - QUICK RECAP                           │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  WHAT: Translates domain names → IP addresses                   │
│                                                                   │
│  HIERARCHY:                                                       │
│    Root (.) → TLD (.com) → Domain (google) → Subdomain (www)    │
│                                                                   │
│  KEY RECORD TYPES:                                                │
│    A     = Domain → IPv4                                         │
│    AAAA  = Domain → IPv6                                         │
│    CNAME = Alias (points to another domain)                      │
│    MX    = Mail server                                           │
│    NS    = Name server for domain                                │
│    TXT   = Text (verification, SPF)                              │
│                                                                   │
│  RESOLUTION FLOW:                                                 │
│    Browser → Local Cache → Resolver → Root → TLD → Auth NS      │
│                                                                   │
│  CACHING:                                                         │
│    TTL = Time To Live (how long to cache)                        │
│    Lower TTL = Faster updates, more DNS queries                  │
│    Higher TTL = Slower updates, fewer queries                    │
│                                                                   │
│  COMMANDS:                                                        │
│    nslookup, dig, host, ping (uses DNS)                          │
│                                                                   │
│  BACKEND USES:                                                    │
│    Service discovery, Load balancing, Geo-routing                │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

## 8. Quiz - Test Your Understanding

### Question 1:
You update your DNS A record to point to a new server IP. Some users still see the old website 2 hours later. Why?

---

### Question 2:
What's the difference between `A` record and `CNAME` record? When would you use each?

---

### Question 3:
Your Kubernetes pod needs to reach `database-service` in the same cluster. Is external DNS involved?

---

### Question 4:
How would you make a domain like `api.myapp.com` route users to the nearest server (India users to India server, US users to US server)?

---

### Question 5:
You run `nslookup mysite.com` and get different IPs each time. Is this a problem?

---

## 9. Answer Key (Check After Attempting)

<details>
<summary>Click to reveal answers</summary>

### Answer 1:
**DNS Caching!**

The old DNS record is cached at multiple levels:
- User's browser cache
- User's OS cache
- User's ISP resolver cache
- Your DNS provider's cache

Each cache respects **TTL (Time To Live)**. If your old TTL was 24 hours, some users will see old IP for up to 24 hours.

**Solution:**
- Before migration: Lower TTL to 5-10 minutes
- Wait for old TTL to expire
- Make the change
- Raise TTL back after migration

### Answer 2:
**A Record:**
- Points domain directly to IP address
- `mysite.com → 192.168.1.100`
- Used for: Root domain, final destination

**CNAME Record:**
- Points domain to ANOTHER domain (alias)
- `www.mysite.com → mysite.com`
- `api.mysite.com → load-balancer.aws.com`
- Used for: Subdomains, CDNs, load balancers

**Key difference:**
- CNAME requires another DNS lookup (domain → domain → IP)
- CNAME cannot be used for root domain (@ symbol)
- A record is the final answer

### Answer 3:
**No, external DNS is not involved!**

Kubernetes has internal DNS (CoreDNS):
- `database-service` resolves inside the cluster
- Full internal domain: `database-service.default.svc.cluster.local`
- This never hits external DNS servers

Flow:
```
Pod → CoreDNS → Kubernetes Service → Pod IP
```

External DNS is only for traffic coming FROM outside the cluster.

### Answer 4:
**Geo DNS / GeoDNS-based load balancing:**

Using AWS Route 53 or CloudFlare:
1. Create **Geolocation routing policy**
2. Configure:
   - Requests from India → Point to Mumbai server IP
   - Requests from US → Point to Virginia server IP
   - Default → Point to nearest/primary server

```
Route 53 Record:
api.myapp.com
├── India     → 13.234.x.x (Mumbai)
├── Americas  → 54.174.x.x (Virginia)
└── Default   → 13.234.x.x (Mumbai)
```

Alternative: Use Anycast DNS (CloudFlare does this) where same IP is announced from multiple locations.

### Answer 5:
**Not necessarily a problem - could be intentional!**

Reasons for different IPs:
1. **Round-Robin DNS**: Load balancing across servers
2. **GeoDNS**: Different IPs for different regions
3. **CDN**: CloudFlare/Akamai return nearest edge server
4. **Health checking**: Unhealthy server removed

**When it IS a problem:**
- If IPs are returning randomly including dead servers
- If you expect consistent results for testing
- If connections are failing intermittently

**Check:** All returned IPs should be valid and responding.

</details>

---

## 🔗 Next Topic: [HTTP/HTTPS Deep Dive](./04-http-https.md)

## 🔙 Previous: [DHCP Protocol](./06-dhcp.md)

---

*Remember: DNS is the first step in EVERY web request. Understand it well!*
