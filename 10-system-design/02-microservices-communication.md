# 🔗 Microservices Communication Patterns

---

## 1. Simple Explanation

In microservices, services need to talk to each other. Two main patterns:

**Synchronous**: Service A calls Service B and WAITS for response (like HTTP request)

**Asynchronous**: Service A sends message and continues. Service B processes later (like email)

---

## 2. Patterns Overview

```
SYNCHRONOUS (Direct):
┌───────┐  HTTP/gRPC  ┌───────┐
│ Svc A │────────────►│ Svc B │
│       │◄────────────│       │
└───────┘  Response   └───────┘
Pros: Simple, immediate response
Cons: Tight coupling, cascading failures

ASYNCHRONOUS (Message Queue):
┌───────┐   Publish   ┌──────────┐  Consume  ┌───────┐
│ Svc A │────────────►│  Queue   │──────────►│ Svc B │
└───────┘             │(RabbitMQ)│           └───────┘
                      └──────────┘
Pros: Decoupled, resilient, scalable
Cons: Eventual consistency, complexity
```

---

## 3. Communication Methods

| Method | Type | Use Case |
|--------|------|----------|
| REST API | Sync | CRUD operations |
| gRPC | Sync | High-perf internal |
| Message Queue | Async | Event processing |
| Event Bus | Async | Pub/sub events |

---

## 4. Practical Example

### Sync - REST between services

```javascript
// Order Service calls Inventory Service
const checkStock = async (productId) => {
  const res = await fetch(`http://inventory-service/stock/${productId}`);
  return res.json();
};
```

### Async - Event-based

```javascript
// Order Service publishes event
const placeOrder = async (order) => {
  await db.save(order);
  await messageQueue.publish('order.created', order);
  // Don't wait for inventory, payment, email...
};

// Inventory Service subscribes
messageQueue.subscribe('order.created', async (order) => {
  await updateStock(order.items);
});
```

---

## 5. Quick Summary

```
Sync (HTTP/gRPC):
- Simple, immediate
- Service must be available
- Use for: User-facing requests

Async (Queue/Events):
- Decoupled, resilient  
- Eventual consistency
- Use for: Background tasks, events

Service Mesh (Istio/Linkerd):
- Handles: Security, retries, load balancing
- Sidecar proxy pattern
```

---

*This pattern is critical for system design interviews!*
