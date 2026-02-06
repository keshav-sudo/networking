# 🔄 Reverse Proxy

---

## 1. What is a Reverse Proxy?

A server that sits in front of your backend servers, handling requests on their behalf.

```
Client → Reverse Proxy → Backend Servers

Client sees: proxy.example.com
Backends: server1:3000, server2:3000 (hidden)
```

---

## 2. Reverse Proxy vs Forward Proxy

```
Forward Proxy (client-side):
Client → Proxy → Internet
"Proxy for clients" (VPN, content filtering)

Reverse Proxy (server-side):
Internet → Proxy → Your Servers
"Proxy for servers" (LB, SSL, caching)
```

---

## 3. Benefits

```
├── Load Balancing: Distribute to multiple backends
├── SSL Termination: Handle HTTPS at proxy
├── Caching: Cache responses
├── Compression: Gzip responses
├── Security: Hide backend IPs
└── Rate Limiting: Protect backends
```

---

## 4. Nginx as Reverse Proxy

```nginx
upstream backend {
    server 127.0.0.1:3000;
    server 127.0.0.1:3001;
}

server {
    listen 80;
    
    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## 5. Quick Summary

```
Reverse Proxy: Front-end for backend servers

Popular: Nginx, HAProxy, Traefik, Caddy

Provides:
├── Load balancing
├── SSL termination
├── Caching
├── Security
└── Single entry point

Every production setup should have one!
```

---

*Next: [CDN](./04-cdn.md)*
