# 🌍 Types of Networks - LAN, WAN, MAN

---

## 1. Simple Explanation (Beginner Level)

Networks come in different **sizes**. Just like you have:
- Your room (small)
- Your house (medium)
- Your city (large)

Similarly, networks are classified by their **geographical size**:

| Type | Full Form | Size | Example |
|------|-----------|------|---------|
| **PAN** | Personal Area Network | Around you (few meters) | Bluetooth headphones connected to phone |
| **LAN** | Local Area Network | Building/Office | Your home WiFi, office network |
| **MAN** | Metropolitan Area Network | City | City-wide cable TV network |
| **WAN** | Wide Area Network | Countries/World | The Internet itself! |

---

## 2. Real-World Analogy

### 🏘️ Think of Communication Systems:

| Network Type | Real-World Equivalent |
|--------------|----------------------|
| **PAN** | Talking to person next to you (few feet) |
| **LAN** | Intercom system in a building - everyone in the building can talk |
| **MAN** | City's local radio station - covers the entire city |
| **WAN** | International phone network - call anyone in the world |

### 🏢 Office Building Analogy:

```
One Desk (PAN):       Your keyboard, mouse, phone connected to laptop
One Floor (LAN):      All computers on 5th floor sharing files & printer
One Building (LAN+):  All floors connected together
Many Buildings (MAN): All company offices in a city connected
Global Offices (WAN): Offices in India, USA, UK all connected
```

---

## 3. Technical Working (Step-by-Step)

### LAN (Local Area Network)

**What it is:** A network limited to a small area (home, office, school)

**Technical Details:**
```
Typical LAN Setup:
                           ┌─────────────┐
                           │   Router    │ ← Connects to Internet
                           └──────┬──────┘
                                  │
                           ┌──────┴──────┐
                           │   Switch    │ ← Connects all devices
                           └──────┬──────┘
                    ┌─────────────┼─────────────┐
                    │             │             │
               ┌────┴────┐  ┌────┴────┐  ┌────┴────┐
               │   PC 1  │  │   PC 2  │  │ Printer │
               └─────────┘  └─────────┘  └─────────┘
```

- **Speed:** Very fast (100 Mbps to 10 Gbps)
- **Technology:** Ethernet, WiFi
- **Ownership:** Single organization
- **Range:** Up to 1-2 km
- **IP Range:** Usually private IPs (192.168.x.x, 10.x.x.x)

**Common LAN IP Ranges:**
```
192.168.0.0   - 192.168.255.255   (Class C Private)
10.0.0.0      - 10.255.255.255    (Class A Private)
172.16.0.0    - 172.31.255.255    (Class B Private)
```

---

### WAN (Wide Area Network)

**What it is:** A network that covers large geographical areas (countries, continents)

**Technical Details:**
```
WAN Connecting Multiple LANs:

  Delhi Office (LAN)              Mumbai Office (LAN)
       ┌─────────┐                     ┌─────────┐
       │ Router  │                     │ Router  │
       └────┬────┘                     └────┬────┘
            │                               │
            │    ┌───────────────────┐     │
            └────│   Internet/WAN    │─────┘
                 │  (Leased Lines,   │
                 │   MPLS, VPN)      │
                 └───────────────────┘
                          │
                    ┌─────┴─────┐
                 USA Office (LAN)
```

- **Speed:** Slower than LAN (1 Mbps to 1 Gbps typically)
- **Technology:** Leased lines, MPLS, Satellite, VPN over internet
- **Ownership:** Multiple organizations (ISPs involved)
- **Range:** Unlimited (entire world)
- **Cost:** Much more expensive than LAN

**The Internet is the world's largest WAN!**

---

### MAN (Metropolitan Area Network)

**What it is:** A network spanning a city or large campus

**Technical Details:**
```
MAN Example - City Network:

    University Campus          City Library           Hospital
         (LAN)                   (LAN)                 (LAN)
          │                        │                     │
          └──────────┬─────────────┼─────────────────────┘
                     │             │
              ┌──────┴─────────────┴──────┐
              │     City Fiber Ring       │
              │    (MAN Infrastructure)   │
              └───────────────────────────┘
```

- **Speed:** Between LAN and WAN (100 Mbps to 10 Gbps)
- **Technology:** Fiber optics, Cable TV networks
- **Range:** 5-50 km (city-sized)
- **Example:** Cable TV networks, University networks across campuses

---

### Comparison Table

| Feature | LAN | MAN | WAN |
|---------|-----|-----|-----|
| **Size** | Room/Building | City | Country/World |
| **Speed** | Very High | High | Variable (often slower) |
| **Error Rate** | Very Low | Low | Higher |
| **Ownership** | Private | Private/Public | Multiple parties |
| **Cost** | Low setup | Medium | High |
| **Latency** | 1-10 ms | 10-50 ms | 50-500+ ms |
| **Example** | Office network | City CCTV | Internet |

---

## 4. Where is This Used in Real Systems?

### In Backend Development:

| Scenario | Network Type | Example |
|----------|--------------|---------|
| **Local development** | LAN (loopback) | `localhost:3000` |
| **Microservices on same server** | LAN | Docker containers talking |
| **Multi-region deployment** | WAN | AWS Mumbai → AWS Virginia |
| **Database replication** | WAN | Primary DB in India, Replica in USA |
| **CDN** | WAN | Content served from nearest edge location |

### In Cloud Architecture:

```
Your Users (Worldwide - WAN)
         │
    ┌────┴────┐
    │   CDN   │  ← Edge locations globally (WAN)
    └────┬────┘
         │
    ┌────┴────┐
    │   LB    │  ← Load Balancer in one region
    └────┬────┘
         │
┌────────┴────────┐
│   VPC (LAN)     │  ← Virtual Private Cloud = Virtual LAN
│                 │
│  ┌─────┐ ┌─────┐│
│  │App 1│ │App 2││  ← Microservices in same LAN
│  └──┬──┘ └──┬──┘│
│     │       │   │
│  ┌──┴───────┴──┐│
│  │   Database  ││
│  └─────────────┘│
└─────────────────┘
```

### AWS Example:
- **VPC** = Your private LAN in cloud
- **Subnets** = Smaller LANs within VPC
- **Internet Gateway** = Connection to WAN (internet)
- **VPC Peering** = Connecting two LANs
- **AWS Direct Connect** = Private WAN connection

---

## 5. Practical Example (Web/Backend)

### Example 1: Checking Your Network Type

```bash
# Check your LAN IP address
ifconfig | grep "inet " | grep -v 127.0.0.1

# Output might show: inet 192.168.1.10
# This 192.168.x.x is a private LAN address

# Check your WAN IP (how internet sees you)
curl ifconfig.me

# Output might show: 49.36.128.45
# This is your public WAN IP
```

### Example 2: Understanding Docker Networking (LAN inside your computer)

```yaml
# docker-compose.yml
version: '3'
services:
  web:
    build: .
    ports:
      - "3000:3000"  # Expose to host LAN
    networks:
      - backend-network  # Internal Docker LAN

  database:
    image: mongo
    networks:
      - backend-network  # Same LAN as web
    # Note: No ports exposed - only accessible within Docker LAN

networks:
  backend-network:
    driver: bridge  # Creates a virtual LAN
```

```javascript
// In your Node.js code
// Connecting to MongoDB in same Docker network (LAN)
mongoose.connect('mongodb://database:27017/mydb');
// Uses hostname 'database' - only works in same network (LAN)
```

### Example 3: Testing Network Latency

```bash
# LAN latency (to your router)
ping 192.168.1.1
# Result: ~1-5ms (very fast - LAN)

# WAN latency (to Google)
ping google.com
# Result: ~30-100ms (WAN, depends on distance)

# WAN latency (intercontinental)
ping google.com.au   # From India to Australia
# Result: ~200-400ms (WAN, very long distance)
```

---

## 6. Common Mistakes & Misunderstandings

| ❌ Mistake | ✅ Correct Understanding |
|-----------|-------------------------|
| "My home WiFi is a WAN" | Home WiFi is a LAN. Wi-Fi is just the medium, not the network type |
| "LAN is always wired" | LAN can be wired (Ethernet) OR wireless (WiFi). Both are LAN |
| "Faster internet = Faster LAN" | Your LAN speed is independent of internet speed. LAN is usually much faster |
| "VPN creates a new network" | VPN creates a virtual connection over existing WAN to simulate being in another LAN |
| "Cloud is a different type of network" | Cloud uses LAN (VPC) and WAN (internet) - same concepts! |

### Interview-Focused Insight:

> **Q: In AWS, what is a VPC and why do we need it?**
>
> **A:** VPC (Virtual Private Cloud) is essentially a **virtual LAN** in the cloud. It creates an isolated network where:
> - Your resources have private IPs
> - They can communicate freely (like office computers)
> - Internet access is controlled through Internet Gateway
> - Security groups act as firewalls
>
> Without VPC, all your AWS resources would be on the public internet!

---

## 7. Quick Summary (Revision Notes)

```
┌──────────────────────────────────────────────────────────────────┐
│                   NETWORK TYPES - QUICK RECAP                     │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  PAN (Personal)    → Bluetooth, USB        → ~10 meters          │
│  LAN (Local)       → Ethernet, WiFi        → ~1-2 km             │
│  MAN (Metro)       → Fiber, Cable          → ~5-50 km            │
│  WAN (Wide)        → Internet, Leased      → Unlimited           │
│                                                                   │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  LAN:  Fast (Gbps), Low latency (ms), Private IPs, Cheap        │
│  WAN:  Slower, High latency, Public IPs, Expensive              │
│                                                                   │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  Backend Context:                                                 │
│    • localhost        = Loopback (self)                          │
│    • Docker network   = Virtual LAN                              │
│    • VPC              = Cloud LAN                                │
│    • Internet         = WAN                                      │
│    • CDN              = Content distributed over WAN             │
│                                                                   │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  Key Formula:                                                     │
│    Distance ↑ = Speed ↓, Latency ↑, Complexity ↑, Cost ↑        │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

## 8. Quiz - Test Your Understanding

Answer these questions in your own words:

### Question 1:
Your Express.js server is running locally and connects to MongoDB Atlas (cloud database). Which network types are involved in this connection?

---

### Question 2:
Why is the latency between microservices in the same AWS VPC (~1ms) much lower than between services in different regions (~100ms)?

---

### Question 3:
A company has offices in Delhi and Mumbai. Each office has its own local network. They want employees in both offices to share files. What type of network connection do they need to set up?

---

### Question 4:
When you run `docker-compose up` with multiple services, and they communicate using service names (like `http://database:27017`), what type of network is Docker creating?

---

### Question 5:
Your application shows 200ms latency when connecting to a database. Your colleague says "Let's move the database to the same server to make it a LAN connection and reduce latency to 1ms." Is this reasoning correct?

---

## 9. Answer Key (Check After Attempting)

<details>
<summary>Click to reveal answers</summary>

### Answer 1:
Multiple network types are involved:
1. **LAN/Loopback**: Your local machine where Express runs
2. **LAN**: Your home/office network (router)
3. **WAN**: The Internet - from your router to MongoDB Atlas
4. **LAN**: MongoDB Atlas's internal cloud network (their VPC)

The request travels: Loopback → Your LAN → WAN (Internet) → MongoDB's LAN

### Answer 2:
This is about **LAN vs WAN**:
- Same VPC = Same virtual LAN
  - Data travels short distance through high-speed AWS internal network
  - Latency: ~0.5-2ms
  
- Different regions = WAN
  - Data travels through internet/AWS backbone across countries
  - Physical distance adds latency (light speed limitation)
  - More network hops (routers) add latency
  - Latency: ~50-200ms depending on distance

### Answer 3:
They need a **WAN connection**. Options include:
1. **VPN over Internet** - Cheapest, uses existing internet
2. **MPLS (Leased Line)** - Private, dedicated connection (expensive)
3. **AWS/Azure VPN** - Cloud-based WAN connection

This creates a connection between two LANs, forming a WAN.

### Answer 4:
Docker creates a **virtual LAN** (specifically a bridge network):
- Each container gets a private IP (usually 172.17.0.x)
- Containers can reach each other by name (DNS provided by Docker)
- This network is isolated from host's network
- It's like creating a mini office network inside your computer

### Answer 5:
The reasoning is **partially correct but needs more context**:

**Correct part**: Yes, if the database and app are on the same machine, network latency will drop to ~0.1ms (essentially negligible).

**What they might be missing**:
- 200ms might not be entirely network latency - could include:
  - Query execution time
  - Connection pool wait time
  - SSL handshake time
  
- If database is already in same region but different server:
  - LAN latency would be ~1-5ms
  - Remaining 195ms is NOT network - investigate query performance

**Better approach**: First diagnose WHERE the 200ms is coming from:
```javascript
console.time('query');
await db.query('SELECT 1');
console.timeEnd('query');  // If this is 2ms, problem is NOT network
```

</details>

---

## 🔗 Next Topic: [Network Topologies](./03-network-topologies.md)

## 🔙 Previous: [What is a Network?](./01-what-is-network.md)

---

*Remember: Understanding LAN vs WAN is fundamental for system design - it affects latency, cost, and architecture decisions!*
