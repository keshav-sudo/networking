# 📡 HTTP/HTTPS - The Web's Foundation

---

## 1. Simple Explanation (Beginner Level)

**HTTP = HyperText Transfer Protocol** - The language browsers and servers use to talk.

**HTTPS = HTTP + Security** - Same thing, but encrypted (nobody can read your data in transit).

```
HTTP:  Like a postcard - anyone can read
HTTPS: Like a sealed envelope - only recipient can read
```

---

## 2. Real-World Analogy

| HTTP Concept | Restaurant Equivalent |
|--------------|----------------------|
| **Client** | Customer |
| **Server** | Kitchen |
| **GET** | "Show me the menu" |
| **POST** | "I want to order this" |
| **PUT** | "Change my entire order" |
| **DELETE** | "Cancel my order" |
| **200 OK** | "Order ready!" |
| **404** | "We don't have that" |
| **500** | "Kitchen on fire!" |

---

## 3. Technical Working

### HTTP Request Structure

```
GET /api/users HTTP/1.1          ← Request Line
Host: api.example.com            ← Headers
Authorization: Bearer token123
Content-Type: application/json

{"name": "John"}                 ← Body (for POST/PUT)
```

### HTTP Response Structure

```
HTTP/1.1 200 OK                  ← Status Line
Content-Type: application/json   ← Headers
Cache-Control: max-age=3600

{"id": 1, "name": "John"}       ← Body
```

### HTTP Methods

| Method | Purpose | Idempotent? |
|--------|---------|-------------|
| **GET** | Read data | Yes |
| **POST** | Create | No |
| **PUT** | Replace | Yes |
| **PATCH** | Partial update | No |
| **DELETE** | Remove | Yes |

### Status Codes

```
2xx - Success ✅
   200 OK, 201 Created, 204 No Content

3xx - Redirect ↪️
   301 Moved Permanently, 302 Found, 304 Not Modified

4xx - Client Error ❌
   400 Bad Request, 401 Unauthorized, 403 Forbidden
   404 Not Found, 429 Too Many Requests

5xx - Server Error 💥
   500 Internal Error, 502 Bad Gateway, 503 Unavailable
```

### HTTPS/TLS Handshake

```
Client                              Server
   │ ─── ClientHello ─────────────► │
   │ ◄── ServerHello + Certificate ─│
   │     [Verify certificate]       │
   │ ─── Key Exchange ────────────► │
   │ ════ Encrypted HTTP ══════════ │
```

---

## 4. Where Used in Real Systems?

Every API you build! REST APIs use HTTP methods:
- `GET /users` - List users
- `POST /users` - Create user  
- `PUT /users/1` - Update user
- `DELETE /users/1` - Remove user

---

## 5. Practical Example

```javascript
// Express.js API
const express = require('express');
const app = express();

app.get('/api/users', (req, res) => {
  res.status(200).json([{id: 1, name: 'John'}]);
});

app.post('/api/users', (req, res) => {
  res.status(201).json({id: 2, ...req.body});
});

app.delete('/api/users/:id', (req, res) => {
  res.status(204).send();
});
```

```bash
# cURL testing
curl -i https://api.example.com/users
curl -X POST -H "Content-Type: application/json" \
  -d '{"name":"John"}' https://api.example.com/users
```

---

## 6. Common Mistakes

| ❌ Mistake | ✅ Correct |
|-----------|-----------|
| "HTTPS is slower" | TLS adds minimal overhead; HTTP/2 is faster |
| "401 = 403" | 401=need login, 403=logged in but no permission |
| "POST for everything" | Use proper methods (GET/PUT/DELETE) |

---

## 7. Quick Summary

```
HTTP: Request-Response protocol for web
HTTPS: HTTP + TLS encryption

Methods: GET(read), POST(create), PUT(replace), DELETE(remove)
Status: 2xx=success, 4xx=client error, 5xx=server error

TLS provides: Encryption, Authentication, Integrity
```

---

## 8. Quiz Questions

1. Your API returns 401 for expired token, 403 for non-admin accessing admin route. Correct?
2. DELETE called twice - should it return 200 both times or 200 then 404?
3. Should search with complex filters use GET or POST?
4. Why can HTTP/2 use one connection while HTTP/1.1 needs multiple?
5. With HTTPS, what can an attacker see? What can't they see?

---

## 9. Answer Key

<details>
<summary>Click to reveal</summary>

1. **Yes!** 401=auth failed, 403=authorized but forbidden
2. **Both OK**: 200 both (idempotent) OR 200 then 404 (informative)
3. **GET** for simple queries, **POST** for complex filter objects
4. **Multiplexing**: HTTP/2 sends parallel requests on one connection; HTTP/1.1 is sequential
5. **Can see**: domain (SNI), timing, size. **Cannot see**: URL path, headers, body, credentials

</details>

---

*Next: [DNS Deep Dive](./05-dns.md)*
