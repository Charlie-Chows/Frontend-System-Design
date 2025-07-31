
## 1. 📦 CDN vs Load Balancer – Key Differences

| Feature              | CDN (Content Delivery Network)                      | Load Balancer                              |
|----------------------|-----------------------------------------------------|---------------------------------------------|
| 📍 Purpose            | Deliver static assets from edge locations          | Distribute traffic across servers           |
| 🌍 Location           | Globally distributed edge servers                  | Regional or within data center              |
| 🗂️ Content Type        | Static (JS, CSS, images, fonts, videos)            | Dynamic (API requests, web pages, DB calls) |
| ⚙️ How it works       | Caches & serves from closest edge node             | Forwards requests based on routing rules    |
| 🚀 Performance Boost  | Reduces latency by serving near user               | Balances server load to avoid overload      |
| 🔐 Security Features  | WAF, DDoS protection, signed URLs, token auth      | SSL termination, IP filtering, health checks |
| 👨‍💻 Managed by         | DevOps / Frontend / Edge team                      | Backend / Infra / DevOps team               |

### ✅ Summary
Use **CDN** to speed up static assets. Use **Load Balancer** to distribute load across backend servers.





## 2. 🧠 CDN Cache vs Browser Cache – Simplified Breakdown

| Aspect              | CDN Cache                                | Browser Cache                            |
|---------------------|-------------------------------------------|-------------------------------------------|
| 🧠 Stored At         | CDN edge server (between user & origin)   | User’s local browser                      |
| 🎯 Content Type      | Static files (JS, CSS, images, fonts)     | All cacheable responses (HTML, CSS, etc.) |
| 📦 Scope             | Shared across users                       | Per user only                             |
| ⏱️ Expiry Control     | Via `Cache-Control`, `Surrogate-Control` | Via `Cache-Control`, `Expires`, ETag      |
| 🔄 Invalidation      | Manual or automated via API               | User has to clear it / browser revalidates |
| ⚡ Speed Benefit     | Faster than origin, still goes over net   | Instant (local)                           |
| 🔐 Risks             | Needs tokenization/auth logic             | May serve stale or unauthorized data      |

### ✅ Summary
- **CDN cache** reduces server load and latency globally.
- **Browser cache** is fastest but user-specific and limited.
