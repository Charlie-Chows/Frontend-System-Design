# Load Balancer – Frontend-Friendly Notes

### 🔹 What is a Load Balancer ?
A Load Balancer is a system or service that distributes incoming traffic across multiple backend servers to:
- A load balancer helps avoid server overload, improve availability, and optimize performance.
**Analogy**
💡 Think of it as a traffic cop that efficiently routes requests to different lanes (servers) to avoid jams.

### 🔹 Why is it important in Frontend ?
Even as a frontend dev, understanding load balancers helps when:
- Debugging production issues (some requests fail, others don’t)
- Dealing with caching, CDNs, and routing logic
- Integrating with reverse proxies or edge platforms
- Handling A/B tests, region-based routing, feature flags, etc.

**1. 🔄 Knowing how requests are routed**
```
fetch("/api/user")
```
- That `/api/user` might go through a load balancer before reaching the backend. So, knowing this helps you:

- Understand how failover or retries work

- Know why some requests might hit slower or different backends

- Spot issues like inconsistent data (e.g., if session data is not shared between backend servers)

**2. 🐛 Debugging Weird Production Bugs**

Let’s say:

- Your app works locally.

- But in production, one server returns incorrect data or errors.

Knowing that a load balancer is randomly picking backend servers helps you realize:

- The bug might not be your frontend code

- It might be one of the backend servers misbehaving

- You can mention this when raising issues with the backend team

**3. 🛡️ Handling Downtime or Errors Gracefully**
- If a server is temporarily down, the load balancer might retry another one — or fail.

You can:

- Add graceful error handling in frontend

- Show “Retry” or “Something went wrong. Try again.”

- Avoid poor UX when server-side issues happen

**4. 🚀 Enabling Performance Techniques**

Some load balancers allow caching or compressing responses before they reach the frontend. Knowing this helps when:

- You want to optimize load time

- You work with DevOps or backend to turn on gzip or brotli compression

- You want to test if load balancer affects performance during deployments

### 🔹 Where does it sit ?
```
React App ── fetch("/api/user") ──▶ Load Balancer ──▶ Server A
                                            |
                                            ├──▶ Server B
                                            |
                                            └──▶ Server C
```

```
[User] → [CDN] → [Load Balancer] → [Server Pool]
```

### 🔹 Types of Load Balancers (based on OSI Layers)

**1. Layer 4 Load Balancer ( Transport Layer – IP & Port level )**
- Works on TCP/UDP level
- Routes based on IP address and port
- Doesn’t look into request content (like path or headers)
- Less flexible, but fast and lightweight.
🧠 Example: Round-robin across server IPs for port 80.

**2. Layer 7 Load Balancer (Application Layer – HTTP-aware )**
- Works on HTTP/HTTPS
- Routes based on application-level data:
    - URL paths (e.g., `/login`, `/dashboard`)
    - Request headers (e.g., device type, auth token)
    - Cookies (e.g., session info)
- More flexible, used in web apps most of the time
🧠 Example: Route `/admin` requests to one server group, and `/user `to another.

### 🔹 How Load Balancers Decide Where to Route?
- Round Robin – sends requests to servers in order
- Least Connections – routes to server with fewest current requests
- IP Hash – sends based on client’s IP (useful for sticky sessions)
- Custom Rules – based on cookies, headers, etc.
✅ These strategies can be combined depending on system architecture.


### Summary or interveiw style answer 
- “A load balancer sits between the user and backend servers, distributing traffic efficiently. There are Layer 4 and Layer 7 types — Layer 7 is especially useful in frontend contexts, where routing is done based on HTTP paths, headers, or cookies. It improves performance, availability, and scalability of web applications.”
- we can achieve zero downtime while update app by redirecting route requests to other servers. So user won't get `500 server error`
