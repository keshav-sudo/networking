# 🔍 Service Discovery

---

## 1. The Problem

In microservices, services need to find each other. But IPs change!

```
Hardcoded: http://192.168.1.10:3000  ← Breaks when IP changes!

Service Discovery: http://user-service  ← Always works
```

---

## 2. How It Works

```
1. Service starts → Registers with registry
   "I'm user-service at 10.0.0.5:3000"

2. Another service needs user-service
   → Asks registry "Where is user-service?"
   → Gets: 10.0.0.5:3000

3. Service stops → Deregisters
   Registry removes it
```

---

## 3. Popular Solutions

| Solution | Type | Use Case |
|----------|------|----------|
| **Kubernetes DNS** | Built-in | K8s environments |
| **Consul** | Registry | Any infrastructure |
| **Eureka** | Registry | Java/Spring |
| **etcd** | Key-value | Distributed config |
| **AWS Cloud Map** | Managed | AWS services |

---

## 4. Kubernetes Service Discovery

```yaml
# Every pod can reach other services by name
apiVersion: v1
kind: Service
metadata:
  name: user-service
spec:
  selector:
    app: user
  ports:
    - port: 80
      targetPort: 3000

# Access via: http://user-service or
# http://user-service.default.svc.cluster.local
```

---

## 5. Quick Summary

```
Service Discovery: Finding services dynamically

Registry Pattern:
├── Services register themselves
├── Clients query registry
└── Health checks remove dead services

In Kubernetes: DNS-based, automatic
Outside K8s: Consul, Eureka, etcd

Critical for: Microservices, auto-scaling
```

---

*Next: [API Gateway](./05-api-gateway.md)*
