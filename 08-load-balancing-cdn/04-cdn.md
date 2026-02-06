# 🌍 CDN - Content Delivery Network

---

## 1. Simple Explanation (Beginner Level)

Imagine you're in India trying to download a file from a server in America. The data travels across the ocean - slow!

**CDN puts copies of your content in servers around the world.** Users get content from the nearest server.

```
Without CDN:
User (India) ──────10,000 km──────► Server (USA)
                   (slow!)

With CDN:
User (India) ──100 km──► CDN Edge (Mumbai) has cached copy
                        (fast!)
```

---

## 2. Real-World Analogy

```
Without CDN = One Pizza Shop
Order from anywhere → Pizza comes from main shop
Far customers wait longer

With CDN = Pizza Chain Franchise
Order → Goes to nearest branch
Everyone gets hot pizza fast!

CDN Edge Server = Local franchise
Origin Server = Main pizza shop
```

---

## 3. Technical Working

### CDN Architecture

```
                    ┌──────────────────┐
                    │  Origin Server   │ ← Your actual server
                    │  (source of      │
                    │   truth)         │
                    └────────┬─────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
    ┌────┴────┐        ┌────┴────┐        ┌────┴────┐
    │ Edge    │        │ Edge    │        │ Edge    │
    │ Mumbai  │        │ London  │        │ Tokyo   │
    └────┬────┘        └────┬────┘        └────┬────┘
         │                   │                   │
    [Users in          [Users in          [Users in
     India]             Europe]            Asia]
```

### How CDN Works

```
1. User requests: cdn.example.com/image.jpg

2. DNS returns nearest edge server IP (GeoDNS)

3. Edge server checks cache:
   - Cached? → Return immediately (cache HIT)
   - Not cached? → Fetch from origin, cache it, return

4. Next request for same file → Served from cache
```

### Cache Headers

```http
# Origin server sets these:
Cache-Control: public, max-age=31536000  
# "Cache for 1 year, anyone can cache"

Cache-Control: private, no-cache
# "Don't cache, always validate"

Cache-Control: no-store
# "Never cache this"
```

---

## 4. Where Used in Real Systems?

| Content Type | CDN Benefit |
|--------------|-------------|
| Images/Videos | Fast loading worldwide |
| Static JS/CSS | Reduced latency |
| API responses | Can cache GET responses |
| Live streaming | Edge handles load |

**Popular CDNs:** Cloudflare, AWS CloudFront, Akamai, Fastly

---

## 5. Practical Example

### CloudFront Setup (AWS)

```javascript
// S3 + CloudFront for static assets
const distribution = {
  origins: [{
    domainName: 'my-bucket.s3.amazonaws.com',
    id: 'myS3Origin'
  }],
  defaultCacheBehavior: {
    targetOriginId: 'myS3Origin',
    viewerProtocolPolicy: 'redirect-to-https',
    cachePolicyId: 'CachingOptimized',
    ttl: { default: 86400 }  // 1 day
  }
};
```

### Cache-Control in Express

```javascript
app.use('/static', express.static('public', {
  maxAge: '1y',  // Cache for 1 year
  etag: true
}));

app.get('/api/products', (req, res) => {
  res.set('Cache-Control', 'public, max-age=300');  // 5 min
  res.json(products);
});
```

### Invalidate CDN Cache

```bash
# AWS CloudFront
aws cloudfront create-invalidation \
  --distribution-id E1234 \
  --paths "/images/*" "/css/*"
```

---

## 6. Common Mistakes

| ❌ Mistake | ✅ Correct |
|-----------|-----------|
| "CDN for all content" | Only cache-able content (not user-specific) |
| "Cache forever" | Set appropriate TTL; invalidate on updates |
| "CDN = Security" | CDN helps, but not complete security solution |

---

## 7. Quick Summary

```
CDN: Distributed servers caching content near users

Benefits:
- Faster load times (reduced latency)
- Less origin server load
- Better availability (edge redundancy)

Key Concepts:
- Edge Server: CDN server near user
- Origin: Your actual server
- Cache-Control: HTTP header for caching rules
- TTL: How long to cache
- Invalidation: Force cache refresh

Good for: Static files, images, videos, cacheable API responses
Bad for: Personalized content, real-time data
```

---

## 8. Quiz Questions

1. Your CDN cached old version of logo.png. How to show new version?
2. User-specific dashboard data - should it be served via CDN?
3. CDN shows stale data for 1 hour after deploy. What's wrong?
4. How does CDN know which edge server is nearest to user?
5. Can CDN cache POST requests?

---

## 9. Answer Key

<details>
<summary>Click to reveal</summary>

1. **Options:**
   - Invalidate cache path `/logo.png`
   - Use versioned filename: `logo-v2.png` or `logo.png?v=2`
   - Lower TTL for frequently changing files

2. **No!** User-specific = personalized = different for each user. CDN would serve wrong data to other users. Set `Cache-Control: private, no-store`.

3. **TTL too high + no invalidation.** Solutions:
   - Lower max-age for frequently updated content
   - Invalidate cache on deploy
   - Use versioned URLs

4. **GeoDNS + Anycast:**
   - DNS returns nearest edge IP based on user location
   - Anycast: Same IP announced from multiple locations; routing finds closest

5. **Generally no.** POST usually modifies data (not cacheable). But some CDNs support caching specific POST responses with proper headers.

</details>

---

*Next: [API Gateway](../09-advanced/05-api-gateway.md)*
