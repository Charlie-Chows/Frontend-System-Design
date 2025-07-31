
# CDN ( Content Delivery Network )

## What Is a CDN?
- A CDN (Content Delivery Network) is a network of servers placed around the world to deliver web content faster to users to improve speed, reliability, and availability.

## Why do we need a CDN in frontend?
When a user from India accesses your app that is hosted in the US:

- Without CDN: Every image, CSS, JS file has to travel from US to India = slow load

- With CDN: Those assets are cached in a nearby CDN server (say, in Mumbai) = faster loading

🔹 CDNs reduce:

- Page load time

- Latency

- Bandwidth cost

- Server load

## How a CDN Works or stores (Step-by-Step)
1. User requests a static asset like main.js from your site.

2. DNS resolves to the nearest CDN edge server.

3. If the asset is cached, it is returned immediately.

4. If not cached, the edge pulls it from the origin server, caches it, then serves it.

5. Subsequent users nearby get it from cache (much faster).

## What Does a CDN Cache?
- ✅ Static Assets: index.html, .js, .css, .png, .jpg, .svg

- ❌ Typically Not Dynamic API Responses (unless configured)

⚠️ Note: You can cache dynamic content using CDNs with cache rules and edge functions (e.g., Cloudflare Workers).


## What kind of content does CDN serve?
Mainly static assets like:

1. JS bundles

2. CSS

3. Images

4. Fonts

5. Videos

Sometimes dynamic content too (advanced CDNs).

### 1. JS bundles

**💡 What:** Your React/Angular/Vue code after build (e.g., `main.243df.js`)

**💾 CDN stores :**

- Minified .js files (e.g., `bundle.js`, `vendor.js`)

- Source maps (optional, e.g., `bundle.js.map`)

**📦 Why cache in CDN?** 
- Because they are large files loaded on every page — caching them on the edge server speeds up delivery.

### 2. CSS

**💡 What:** Stylesheets like `main.css`, `theme.css`

**💾 CDN stores :**

- Minified .css files 

- Preprocessed files (like `.scss`) after they’re compiled

**📦 Why cache in CDN?** 
- Users load these on every page, and they're usually shared across the site.

### 3. Images

**💡 What:** Any asset in `.png`, `.jpg`, `.svg`, `.webp`

**💾 CDN stores :**

- Optimized versions (compressed/resized variants)

- Responsive formats (`image@2x.jpg`, etc.)

**📦 Why cache in CDN?** 
- Images are large in size and don’t change often → perfect for caching.

### 4. Fonts

**💡 What:**  `.woff`, `.woff2`, `.ttf`, `.eot` font files

**💾 CDN stores :**

- All font file formats for cross-browser support


**📦 Why cache in CDN?** 
- Fonts are rarely updated and are reused across the site.


### 5. Videos & Media Files

**💡 What:**  `.mp4`, `.webm`, audio files

**💾 CDN stores :**

- Compressed media files
- Multiple resolutions/bitrates if you use adaptive streaming

**📦 Why cache in CDN?** 
- Large bandwidth files → offload from your server → prevent lag for users.

### What about dynamic content?
Usually, dynamic content is NOT cached, because it changes per user.

But advanced CDNs can:

- Cache dynamic API responses with headers like Cache-Control

- Use edge functions or CDN workers to render some dynamic logic near the user

- e.g., Cloudflare Workers, Netlify Edge Functions

- Enable full-page caching for dynamic SSR pages (like Next.js with fallback strategies)


## Why CDNs Matter (Especially for Frontend Developers)

- 🌍 Reduced Latency: Content is served from the nearest edge location.

- 🚀 Faster Load Times: Quicker first paint and interaction.

- 🛡️ DDoS Protection: Many CDNs absorb attack traffic at the edge.

- 📈 Scalability: Offloads traffic from your origin server.

- 🧠 Smart Caching: CDNs cache frequently accessed files, reducing repeat load on servers.

- 💵 Cost Efficient: Reduces origin server bandwidth usage.



## When Do Frontend Developers Use or Care About CDNs?
- Deploying frontend apps to Vercel/Netlify = using CDN out of the box.

- Using lazy-loading + image CDNs for fast delivery (e.g., Next.js Image Optimization with CDN).

- Adding CDN to custom domains via Cloudflare or CloudFront.

- Configuring cache headers and invalidations for asset freshness.

- Reducing LCP, FCP, TTFB → impacts Core Web Vitals.

## Terms You Should Know
- **Edge Server**: Server in CDN network closest to the user.

- **Origin Server**: Your main application server.

- **Cache Invalidation**: Manually or automatically removing old cache.

- **TTL (Time To Live)**: How long content stays cached.

- **Purge**: Force-remove something from cache.

- **Edge Functions**: Run logic close to user (Cloudflare Workers, etc.).

## CDN Invalidation
### What is it?
- CDN invalidation is the process of removing or marking specific cached assets as stale on the CDN so that fresh content is fetched from the origin server next time.

### Why it's needed ?
- When you deploy new code (JS/CSS/images), CDNs may still serve the old version from their cache.

- You need invalidation if:

    - You reuse filenames (like main.js instead of hashed names).

    - You're pushing hotfixes and want the change to reflect immediately.

### How it works ?
- You send an invalidation request to the CDN (e.g., `/*` or `/main.js`).

- The CDN purges or marks those files stale.

- On next request, it pulls fresh content from origin, then re-caches it.

### Best Practices 
## 🧠 Best Practices

| Tip                          | Explanation                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| 🔁 Use hashed filenames      | Avoid the need for invalidation altogether. Eg: `/main.abc123.js`          |
| 🧹 Invalidate selectively    | Avoid full purge (`/*`) unless critical. Use path-level targeting.         |
| ⚡ Automate with CI/CD       | Automate invalidation post-deploy using APIs/scripts                        |
| ⏱️ Watch invalidation limits | Some CDNs (like CloudFront) have rate/batch limits                          |

### Quick Example (CloudFront) 
```
aws cloudfront create-invalidation \
  --distribution-id <ID> \
  --paths "/main.js" "/style.css"
```

## 🛡️ CDN Security – Key Concepts (SDE-2 Level)

### 🔐 1. HTTPS (SSL/TLS)
- **What:** Encrypts data between the CDN and client.
- **Why:** Prevents man-in-the-middle attacks and ensures data integrity.
- **Note:** Make sure both origin → CDN and CDN → client use HTTPS.

---

### 🔑 2. Signed URLs / Signed Cookies
- **What:** Generate time-limited, user-specific access tokens for CDN content.
- **Why:** Protect premium/authorized content like videos or paid assets.

---

### 🌐 3. Restrict Origin Access
- **What:** Only allow the CDN to access your origin server (via IP whitelisting).
- **Why:** Prevents bypassing CDN and directly hitting your server.

---

### 🧱 4. WAF (Web Application Firewall)
- **What:** CDN-integrated firewall that filters malicious traffic (e.g., SQLi, XSS).
- **Why:** Protects your app from common web exploits at the CDN edge.

---

### 📈 5. Rate Limiting / DDoS Protection
- **What:** Limits the number of requests from an IP or region.
- **Why:** Prevents abuse, scraping, and large-scale denial-of-service attacks.

---

### 🧾 6. CORS Headers
- **What:** Properly configure `Access-Control-Allow-Origin`.
- **Why:** Controls which domains can access your CDN-hosted resources.

---

### 🔎 7. Token Authentication (JWT, OAuth)
- **What:** Validate access with tokens before serving protected assets.
- **Why:** Secure API or CDN access for authenticated users only.

---

### ✅ Summary for SDE-2 Interviews
- Always **encrypt traffic** (HTTPS), **secure access** (signed URLs), and **protect origin**.
- Implement **rate limiting, WAF**, and **authorization** for sensitive content.
- Automate & monitor CDN security rules as part of CI/CD.


## 📦 Cache-Control & CDN Caching
### 🔹 What is Cache-Control?
- An HTTP response header used to tell browsers and CDNs what to cache and for how long.

### 🔹 Who sets it?
Not set inside your React/JS code. It’s configured in:
- ✅ Server (e.g., Express.js, Nginx)

- ✅ CDN (e.g., Cloudflare, AWS CloudFront)

- ✅ Static hosting config (e.g., Vercel, Netlify)

### 🔹 Why does it matter for frontend devs?
Because:

- You ship JS, CSS, images, fonts

- You want them to be cached efficiently

- You want users to always see the latest version, but not reload everything unnecessarily

### 🔹 Common Example Header
```
Cache-Control: public, max-age=31536000, immutable
```
**Meaning:**

- public: can be cached by browser + CDN

- max-age=31536000: cache for 1 year (in seconds)

- immutable: file won’t change → skip revalidation

### 🔹 Typical Use Case: Hashed Assets
When you build your frontend app (React, etc.), filenames are like:
```
main.8f9e1234.js
style.4cfa23d.css
```
👉 These hashed filenames make files safe to cache forever.
You can then safely apply:
```
Cache-Control: public, max-age=31536000, immutable
```
🧠 Because if content changes → filename changes → browser/CDN fetches new one.

### 🔹 Where to set it?
**Express.js (Node backend):**
```
app.use(express.static("build", {
  maxAge: "1y" // Adds Cache-Control header
}));

```
**vercel :**
```
"headers": [
  {
    "source": "/static/(.*)",
    "headers": [
      { "key": "Cache-Control", "value": "public, max-age=31536000, immutable" }
    ]
  }
]
```
### Summary of Cache-Control
| Concept         | Explanation                                        |
|----------------|----------------------------------------------------|
| Cache-Control  | HTTP header to tell browsers/CDNs how to cache     |
| Set where?     | CDN config, server, or static host settings         |
| What it affects?| JS, CSS, images, fonts (static assets)             |
| Best practice  | Use hashed filenames + long cache + immutable       |

**🔹 In Interviews (How to Say It) :**
- As a frontend engineer, I don't write `Cache-Control` directly in the code, but I understand how to configure it in the CDN or hosting platform to ensure static assets are aggressively cached and versioned correctly. This helps with performance and prevents stale files from being served.

## Summary 
CDN (Content Delivery Network) is a globally distributed network of servers used to deliver static content like JavaScript bundles, CSS files, fonts, images, and other media.

When a user requests a site for the first time, the CDN node (nearest to the user) may not have the requested file — so it fetches it from the origin server (cache miss), stores it (caches it), and serves it to the user.

On subsequent requests, the CDN serves the cached content directly from the nearest edge location (cache hit), improving load times and reducing traffic to the origin.

We can configure caching behavior using TTL (Time-To-Live) headers like Cache-Control or CDN dashboard settings.

This results in faster user experience, less load on your server, and better scalability.