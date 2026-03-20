# Content Delivery Network (CDN)

## Definition

A **CDN (Content Delivery Network)** is a distributed network of servers used to cache and deliver content (mainly static data) from locations closer to the user.

👉 CDN = Group of servers placed in different regions  
👉 Used to reduce latency and improve performance  

---

## Why Do We Need CDN?

Let’s understand with an example.

---

## Example: Netflix System 🎬

Suppose users from different regions:

- USA  
- India  
- Japan  
- UK  

All users try to access:  
www.netflix.com  

---

### Without CDN

1. User requests www.netflix.com  
2. DNS resolves it to Netflix server IP  
3. Request goes to Netflix server (hosted in USA)  
4. Server fetches video from storage (like S3)  
5. Video is sent back to user  

### Problem

- Users near USA → Fast response  
- Users far away (India, Japan) → High latency  

Why?

Because:
- Data travels long distance  
- Network delay increases  

---

## Additional Problems

### 1. High Latency

- Users far from server face delays  
- Poor streaming experience  

---

### 2. Scalability Issue

- All users hit the same origin server  
- Server becomes overloaded  

---

### 3. Access Control / Geo Restrictions

Example:

- Some movies are banned in India  
- Netflix must restrict content based on location  

If everything is stored in one server (USA),  
handling region-based content becomes complex.

---

## Solution: CDN

CDN solves these problems by caching data closer to users.

---

## How CDN Works

Instead of serving all requests from one central server:

- We deploy CDN servers in multiple regions  
- These servers store (cache) frequently accessed data  

---

### Example with CDN

Now we have CDN servers in:

- USA  
- India  
- UK  

User flow:

1. User requests www.netflix.com  
2. DNS routes request to nearest CDN  
3. CDN checks if data is available  

### Case 1: Cache Hit

- Data is already in CDN  
- CDN directly serves the content  

👉 Fast response  

---

### Case 2: Cache Miss

- CDN does not have data  
- CDN requests data from origin server (USA)  
- Stores it (cache)  
- Sends response to user  

👉 Next request becomes fast  

---

## Important Concept: Not All Data is Stored

CDN does NOT store all data.

- Only frequently accessed (popular) content is cached  
- Less-used content stays in origin server  

---

## Point of Presence (PoP)

The location where CDN servers are deployed is called:

👉 **Point of Presence (PoP)**

Example:

- India CDN → PoP  
- UK CDN → PoP  

---

## What if No CDN in a Region?

Example:

- No CDN in Japan  

Then:

- Request goes to nearest CDN (e.g., India or UK)  
- That CDN serves the request  

---

## DNS Routing in CDN

DNS plays a key role.

Instead of returning one fixed IP:

- DNS returns IP of nearest CDN  

This is called:

👉 **Geo-based Routing**

Example:

- User in India → Indian CDN  
- User in UK → UK CDN  

---

## Anycast Routing

CDNs also use **Anycast Routing**.

In Anycast:

- Same IP address is assigned to multiple CDN servers  
- Network automatically routes user to nearest server  

👉 Improves performance and reliability  

---

## Types of CDN Layers

- Origin Server (Main storage like S3)  
- Regional CDN  
- Edge CDN (closest to user)  

Flow:

User → Edge CDN → Regional CDN → Origin Server  

---

## Advantages of CDN

### 1. Low Latency

- Data served from nearest location  
- Faster response time  

---

### 2. Better Performance

- Faster loading of videos, images, static content  

---

### 3. Reduced Load on Origin Server

- CDN handles most requests  
- Origin server is protected  

---

### 4. High Availability

- If one CDN fails → another CDN serves  

---

### 5. Geo-based Content Control

- Serve region-specific content  
- Block restricted content  

---

## Challenges

- Cache invalidation is difficult  
- Data consistency across regions  
- Infrastructure cost  
- Not suitable for dynamic data  

---

## Summary

A CDN is a distributed system that caches content closer to users.

It helps in:

- Reducing latency  
- Improving performance  
- Handling large traffic  
- Providing geo-based content  

CDN is widely used in systems like:

- Netflix  
- YouTube  
- Amazon  

---

## Interview Explanation (2–3 Minutes)

A CDN is a distributed network of servers that caches static content closer to users to reduce latency.

For example, in a system like Netflix, if all data is served from a US server, users from India or Japan will experience high latency.

To solve this, CDN servers are deployed in multiple regions. When a user makes a request, DNS routes it to the nearest CDN. If the content is cached, it is served directly. Otherwise, the CDN fetches it from the origin server and caches it for future use.

CDNs also use techniques like geo-based DNS routing and Anycast routing to ensure users connect to the nearest server.

Overall, CDN improves performance, reduces latency, and decreases load on the origin server.
