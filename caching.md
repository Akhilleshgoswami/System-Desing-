```md id="distributed-caching"
# Distributed Caching: What, Why, and How

Caching is a technique to **store frequently accessed data in memory** so that future requests for that data can be served faster, avoiding repeated expensive operations like database reads.

---

# Why Do We Need Caching?

Imagine using an **e-commerce application**:

- User frequently searches for items or checks their profile  
- Every request goes: **Client → Server → Database → Server → Client**  
- Each DB operation is **expensive and slow** (10–13 ms round trip)  
- High traffic = DB overload, slower responses  

### Solution: Cache

- Store frequently used data in **memory**  
- First, check the cache:  
  - If present → return instantly  
  - If not → fetch from DB, store in cache, then return  

**Benefits:**

- Reduces network calls  
- Avoids repeated computations  
- Decreases DB load  
- Faster request-response  

---

# Local Caching

- Each application server maintains its **own cache**  
- Problem: **Data inconsistency** across multiple servers  

---

# Distributed Caching

- A **global cache** shared among all application servers  
- Ensures **data consistency** and reduces redundant caching  

### Example

- Application has 3 servers  
- All servers read/write to a **shared Redis or Memcached cluster**  
- Frequently accessed items (e.g., top-selling products) stay in the cache for all servers  

---

# Cache Policies

### 1️⃣ Data Eviction Policies

To manage memory efficiently, caches remove old data using **eviction policies**:

| Policy | How it works | Example |
|--------|-------------|--------|
| FIFO (First In First Out) | Removes oldest item first | Items added on Day 1 removed before Day 2 items |
| LRU (Least Recently Used) | Removes item not used recently | Items not accessed in last 2 days removed first |
| LFU (Least Frequently Used) | Removes least accessed item | Items accessed only once removed before frequently accessed ones |

- Data can also be removed after a **time interval** (e.g., 5 days)  
- Problem with FIFO: may remove important data that is frequently accessed  

---

### 2️⃣ Cache Write Policies

Decide how updates are reflected in cache:

- **Write-Through:**  
  - Write goes to **cache + DB simultaneously**  
  - Ensures consistency  
  - Slower writes  

- **Write-Back:**  
  - Write goes to **cache first**, DB updated later asynchronously  
  - Faster writes  
  - Risk of losing data if cache crashes  

- **Write-Around:**  
  - Write goes **only to DB**, cache updated on next read  
  - Reduces cache pollution  
  - Slower for first read after write  

---

# Summary

- **Caching** reduces DB load, improves speed, avoids repeated computations  
- **Local caching:** simple, but can cause inconsistency across servers  
- **Distributed caching:** global memory shared among servers → consistent and fast  
- **Eviction policies:** FIFO, LRU, LFU to remove stale data  
- **Write policies:** Write-Through, Write-Back, Write-Around  

**Use Case Example:**  
- Redis or Memcached used as distributed cache for e-commerce apps  
- Frequently accessed product info or user profile stored in cache  
- Avoids multiple DB reads for high-traffic systems  

Caching is a **must-have** for scaling modern applications efficiently.
```

