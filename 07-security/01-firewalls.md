# 🔥 Firewalls

---

## 1. Simple Explanation (Beginner Level)

A **Firewall** is like a security guard at a building entrance:
- Checks who's coming in and going out
- Has a list of allowed/blocked people (rules)
- Can work at the entrance (network edge) or inside (host-based)

In networking: A firewall filters network traffic based on rules you define.

---

## 2. Real-World Analogy

```
Airport Security = Firewall

✓ Allow: Ticketed passengers (authorized traffic)
✗ Block: No ticket (unauthorized access)
✓ Allow: Staff with badge (internal traffic)
✗ Block: Prohibited items (malicious traffic)

Rules:
- Port 443 open = "Business class passengers allowed"
- Port 22 from specific IP = "Only VIP lounge members"
- All other ports closed = "No unauthorized entry"
```

---

## 3. Technical Working

### Types of Firewalls

```
1. Packet Filter (Stateless) - Layer 3/4
   Checks: Source/Dest IP, Source/Dest Port, Protocol
   Fast but dumb (doesn't track connections)

2. Stateful Firewall - Layer 3/4
   Tracks connection state
   "This reply packet matches an outgoing request"

3. Application Firewall (WAF) - Layer 7
   Inspects HTTP content
   Blocks SQL injection, XSS attacks
```

### Firewall Rules Example

```
┌────────────────────────────────────────────────────────────┐
│ Rule# │ Source      │ Dest      │ Port  │ Protocol │ Action│
├───────┼─────────────┼───────────┼───────┼──────────┼───────┤
│ 1     │ Any         │ 10.0.0.5  │ 443   │ TCP      │ ALLOW │
│ 2     │ 10.0.0.0/24 │ 10.0.0.5  │ 22    │ TCP      │ ALLOW │
│ 3     │ Any         │ 10.0.0.5  │ 22    │ TCP      │ DENY  │
│ 4     │ Any         │ Any       │ Any   │ Any      │ DENY  │
└────────────────────────────────────────────────────────────┘

Order matters! Rules checked top-to-bottom.
Rule 4 = Default deny (whitelist approach)
```

### Inbound vs Outbound

```
INBOUND (incoming):
Internet → [Firewall] → Your Server
"Who can access my server?"

OUTBOUND (outgoing):
Your Server → [Firewall] → Internet
"What can my server access?"
```

---

## 4. Where Used in Real Systems?

| Type | Example |
|------|---------|
| Cloud Security Groups | AWS SG, Azure NSG |
| Host Firewall | iptables, ufw, Windows Firewall |
| Network Firewall | Cisco ASA, pfSense |
| WAF | AWS WAF, Cloudflare, ModSecurity |

---

## 5. Practical Example

### Linux iptables

```bash
# Allow incoming SSH from specific IP
iptables -A INPUT -p tcp -s 203.0.113.5 --dport 22 -j ACCEPT

# Allow incoming HTTPS from anywhere
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Allow established connections (stateful)
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Drop everything else
iptables -A INPUT -j DROP

# List rules
iptables -L -n
```

### UFW (Simpler)

```bash
# Enable firewall
sudo ufw enable

# Allow SSH and HTTPS
sudo ufw allow 22/tcp
sudo ufw allow 443/tcp

# Allow from specific IP
sudo ufw allow from 10.0.0.0/24 to any port 3306

# Check status
sudo ufw status
```

### AWS Security Group (via CLI)

```bash
# Allow HTTPS from anywhere
aws ec2 authorize-security-group-ingress \
  --group-id sg-12345 \
  --protocol tcp \
  --port 443 \
  --cidr 0.0.0.0/0

# Allow SSH from your IP only
aws ec2 authorize-security-group-ingress \
  --group-id sg-12345 \
  --protocol tcp \
  --port 22 \
  --cidr 203.0.113.5/32
```

---

## 6. Common Mistakes

| ❌ Mistake | ✅ Correct |
|-----------|-----------|
| "Open all ports for testing" | Opens to attacks; use specific rules |
| "Firewall = complete security" | Part of defense-in-depth only |
| "Block port = block protocol" | HTTP can tunnel through 443 |

---

## 7. Quick Summary

```
Firewall: Network traffic filter based on rules

Types:
- Packet Filter: IP/port based (fast, simple)
- Stateful: Tracks connections
- WAF: Application-layer inspection

Key Concepts:
- Inbound vs Outbound rules
- Default deny (whitelist) is safer
- Rules processed top-to-bottom

Cloud: Security Groups work as firewalls
```

---

## 8. Quiz Questions

1. Server needs HTTP, HTTPS, SSH (admin IP only). What rules?
2. You blocked port 22, but someone SSHs via port 443. How?
3. Stateless vs Stateful firewall for web server?
4. Your app works locally but fails in cloud. What to check?
5. Can firewall prevent SQL injection?

---

## 9. Answer Key

<details>
<summary>Click to reveal</summary>

1. ```
   Allow TCP 80 from 0.0.0.0/0
   Allow TCP 443 from 0.0.0.0/0
   Allow TCP 22 from AdminIP/32
   Deny all else
   ```

2. **SSH tunneling over HTTPS port!** Block by port != block by protocol. Need application-layer inspection.

3. **Stateful**: Web traffic needs return packets. Stateful auto-allows responses to outgoing requests.

4. **Security Group/Firewall rules!** Cloud instances are locked down by default. Check inbound rules for required ports.

5. **Basic firewall: No.** Need WAF (Web Application Firewall) which inspects HTTP content for SQL patterns.

</details>

---

*Next: [TLS/SSL Handshake](./02-tls-ssl.md)*
