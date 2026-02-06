# ⚖️ Load Balancing

---

## 1. Simple Explanation (Beginner Level)

Imagine a restaurant with only ONE waiter serving 100 customers. Everyone waits forever!

Now imagine 5 waiters, each serving 20 customers. Much faster!

**Load Balancer = The host who directs customers to different waiters**

In web terms: A load balancer distributes incoming requests across multiple servers so no single server gets overwhelmed.

---

## 2. Real-World Analogy

```
Without Load Balancer:          With Load Balancer:
                                
   👤👤👤👤                         👤👤👤👤
      │                              │
      ▼                        ┌─────┴─────┐
   [Server]                    │    LB     │
   (overloaded!)               └─┬───┬───┬─┘
                                 │   │   │
                                 ▼   ▼   ▼
                               [S1][S2][S3]
                               (balanced!)
```

**Bank Teller Analogy:**
- Load Balancer = The person who says "Next! Counter 3 is free"
- Servers = Bank counters
- Users = Customers in queue

---

## 3. Technical Working

### Load Balancer Position in Architecture

```
Internet
    │
    ▼
┌─────────────────┐
│  Load Balancer  │  ← Single entry point
│  (Public IP)    │
└────────┬────────┘
         │
    ┌────┼────┐
    ▼    ▼    ▼
┌─────┐┌─────┐┌─────┐
│App 1││App 2││App 3│  ← Private IPs
└─────┘└─────┘└─────┘
```

### Load Balancing Algorithms

| Algorithm | How It Works | Best For |
|-----------|--------------|----------|
| **Round Robin** | Server 1, 2, 3, 1, 2, 3... | Equal capacity servers |
| **Weighted Round Robin** | More requests to stronger servers | Mixed capacity |
| **Least Connections** | Send to server with fewest active connections | Variable request duration |
| **IP Hash** | Same client IP → same server | Session persistence |
| **Least Response Time** | Send to fastest responding server | Performance optimization |

### L4 vs L7 Load Balancers

```
L4 (Transport Layer):
┌────────────────────────────────────┐
│ Sees: IP + Port                    │
│ Fast, simple                       │
│ Can't see HTTP headers/content     │
│ Example: AWS NLB, HAProxy TCP mode │
└────────────────────────────────────┘

L7 (Application Layer):
┌────────────────────────────────────┐
│ Sees: Full HTTP request            │
│ URL, headers, cookies              │
│ Smart routing decisions            │
│ Example: AWS ALB, Nginx, HAProxy   │
└────────────────────────────────────┘
```

### Health Checks

```
Load Balancer continuously checks:
"Are you alive?"

┌────┐     GET /health       ┌────┐
│ LB │ ─────────────────────▶│ S1 │  200 OK ✓
└────┘                       └────┘

┌────┐     GET /health       ┌────┐
│ LB │ ─────────────────────▶│ S2 │  500 Error ✗
└────┘                       └────┘
                             (removed from pool)
```

---

## 4. Where Used in Real Systems?

| Service | Load Balancer |
|---------|---------------|
| AWS | ALB (L7), NLB (L4), CLB |
| GCP | Cloud Load Balancing |
| Nginx | Can act as LB |
| HAProxy | Popular open-source LB |
| Kubernetes | Services with kube-proxy |

---

## 5. Practical Example

### Nginx Load Balancer Config

```nginx
# /etc/nginx/nginx.conf
upstream backend {
    least_conn;  # Algorithm
    
    server 10.0.0.1:3000 weight=3;  # Gets 3x traffic
    server 10.0.0.2:3000 weight=1;
    server 10.0.0.3:3000 backup;     # Only if others fail
}

server {
    listen 80;
    
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
    
    location /health {
        return 200 'OK';
    }
}
```

### Express.js Health Endpoint

```javascript
app.get('/health', (req, res) => {
  // Check dependencies
  const dbHealthy = checkDatabaseConnection();
  const cacheHealthy = checkRedisConnection();
  
  if (dbHealthy && cacheHealthy) {
    res.status(200).json({ status: 'healthy' });
  } else {
    res.status(503).json({ status: 'unhealthy' });
  }
});
```

---

## 6. Common Mistakes

| ❌ Mistake | ✅ Correct |
|-----------|-----------|
| "LB eliminates single point of failure" | LB itself can fail! Need redundant LBs |
| "All algorithms are same" | Choose based on your traffic pattern |
| "L4 can do URL-based routing" | Only L7 can see URL/headers |

---

## 7. Quick Summary

```
Load Balancer: Distributes traffic across servers

Algorithms:
- Round Robin: Equal distribution
- Least Connections: To least busy server
- IP Hash: Same client → same server

Types:
- L4: Fast, sees IP/port only
- L7: Smart, sees HTTP details

Health Checks: Remove unhealthy servers

Always have: Redundant LBs for HA
```

---

## 8. Quiz Questions

1. 3 servers, Round Robin. After 7 requests, which server gets the 8th?
2. User login stored in server memory. Which algorithm maintains session?
3. Can L4 load balancer route `/api` to Server A and `/images` to Server B?
4. Server fails health check. What happens to in-flight requests?
5. Why might Least Connections be better than Round Robin?

---

## 9. Answer Key

<details>
<summary>Click to reveal</summary>

1. **Server 2**. Sequence: 1,2,3,1,2,3,1 → next is 2.

2. **IP Hash** or **Sticky Sessions**. Same client IP goes to same server. Or: store sessions in Redis (shared state).

3. **No!** L4 only sees IP/port. Use L7 for path-based routing.

4. **Depends**: In-flight requests may fail. Good LB does connection draining - waits for active requests to complete before removing server.

5. **Variable request duration**: If some requests take 100ms and others take 10s, Round Robin could overload servers with slow requests. Least Connections adapts naturally.

</details>

---

*Next: [Load Balancing Algorithms](./02-lb-algorithms.md)*
