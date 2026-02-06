# 🏗 System Design Networking Patterns

---

## 1. High Availability Patterns

```
Multi-AZ Deployment:
┌─────────────────┐  ┌─────────────────┐
│      AZ-1       │  │      AZ-2       │
│ ┌─────────────┐ │  │ ┌─────────────┐ │
│ │   Server    │ │  │ │   Server    │ │
│ │   (Active)  │ │  │ │  (Standby)  │ │
│ └─────────────┘ │  │ └─────────────┘ │
│ ┌─────────────┐ │  │ ┌─────────────┐ │
│ │   Database  │ │  │ │   Replica   │ │
│ │   (Primary) │ │  │ │             │ │
│ └─────────────┘ │  │ └─────────────┘ │
└─────────────────┘  └─────────────────┘
```

---

## 2. Typical Web Architecture

```
Users
  │
  ↓
┌──────────────┐
│     CDN      │  ← Static assets
└──────┬───────┘
       ↓
┌──────────────┐
│ API Gateway  │  ← Auth, rate limit
└──────┬───────┘
       ↓
┌──────────────┐
│ Load Balancer│
└──────┬───────┘
       ↓
┌──────┼──────┐
│ App  │ App  │  ← Stateless
└──────┬──────┘
       ↓
┌──────────────┐
│    Cache     │  ← Redis
└──────┬───────┘
       ↓
┌──────────────┐
│   Database   │  ← PostgreSQL
└──────────────┘
```

---

## 3. Database Patterns

```
Read Replicas:
Write → Primary DB
Read  → Replicas (distributed)

Sharding:
Users 1-1000 → Shard 1
Users 1001-2000 → Shard 2

Connection Pooling:
App → Pool → Database
(Reuse connections, don't create new each time)
```

---

## 4. Caching Strategies

```
Cache-Aside:
1. Check cache
2. Cache miss? → Read from DB → Update cache
3. Cache hit? → Return cached data

Write-Through:
1. Write to cache
2. Cache writes to DB

Write-Behind:
1. Write to cache
2. Cache async writes to DB
```

---

## 5. Quick Summary

```
HA: Multi-AZ, replicas, failover
LB: Distribute load, health checks
CDN: Edge caching for static content
Cache: Redis/Memcached for hot data
DB: Replicas, sharding, connection pools

Design for failure:
├── Redundancy at every layer
├── Health checks everywhere  
├── Graceful degradation
└── Auto-scaling
```

---

*Use these patterns in system design interviews!*
