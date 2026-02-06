# 🚪 API Gateway

---

## 1. What is an API Gateway?

Single entry point for all client requests. Routes to appropriate backend services.

```
Without Gateway:
Client → Service A
Client → Service B
Client → Service C

With Gateway:
Client → API Gateway → Service A
                    → Service B
                    → Service C
```

---

## 2. Functions

```
├── Routing: /users → user-service
├── Authentication: Verify JWT tokens
├── Rate Limiting: 1000 req/min
├── Load Balancing: Distribute requests
├── Caching: Cache responses
├── Transformation: Request/response changes
└── Monitoring: Log all requests
```

---

## 3. Popular API Gateways

| Gateway | Best For |
|---------|----------|
| **Kong** | Plugin ecosystem |
| **AWS API Gateway** | Serverless |
| **Nginx** | Performance |
| **Traefik** | Kubernetes |
| **Express Gateway** | Node.js |

---

## 4. Example: Kong Config

```yaml
services:
  - name: user-service
    url: http://user-service:3000
    routes:
      - name: user-route
        paths:
          - /api/users
    plugins:
      - name: rate-limiting
        config:
          minute: 100
      - name: jwt
```

---

## 5. Quick Summary

```
API Gateway: Single entry point for clients

Provides:
├── Central routing
├── Authentication/Authorization
├── Rate limiting
├── Load balancing
├── Request transformation
└── Monitoring

Popular: Kong, AWS API Gateway, Nginx

Every microservices architecture needs one!
```

---

*This completes the Advanced Networking module!*
