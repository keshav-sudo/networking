# ⚖️ Load Balancing Algorithms

---

## 1. Common Algorithms

### Round Robin
```
Request 1 → Server A
Request 2 → Server B
Request 3 → Server C
Request 4 → Server A (repeat)
```
Best for: Equal capacity servers

### Weighted Round Robin
```
Server A (weight 3): gets 3 requests
Server B (weight 1): gets 1 request

For every 4 requests: A, A, A, B
```
Best for: Unequal server capacities

### Least Connections
```
Server A: 10 active connections
Server B: 5 active connections

New request → Server B (fewer connections)
```
Best for: Variable request durations

### IP Hash
```
hash(client_IP) % num_servers = server_index

Same client always goes to same server
```
Best for: Session persistence needed

---

## 2. Comparison

| Algorithm | Session | Complexity | Use Case |
|-----------|---------|------------|----------|
| Round Robin | No | Low | Simple distribution |
| Weighted | No | Low | Mixed capacity |
| Least Conn | No | Medium | Variable duration |
| IP Hash | Yes | Low | Sticky sessions |

---

## 3. Health Checks

```
Every 10 seconds:
LB → GET /health → Server

200 OK → Keep in pool
500/timeout → Remove from pool
```

---

## 4. Quick Summary

```
Round Robin: Simple, equal distribution
Weighted: Different capacity servers
Least Connections: Adaptive to load
IP Hash: Same client → same server

Always combine with health checks
Consider session requirements
```

---

*Next: [Reverse Proxy](./03-reverse-proxy.md)*
