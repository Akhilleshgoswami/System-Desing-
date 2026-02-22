
---

# Load Balancing in Microservices (L4 + L7 + Algorithms)

## Why Load Balancer is Needed

In microservices architecture, each service can scale independently.

Example:

* ChatService → multiple instances
* UserService → multiple instances

Now we must decide:

1. Which service should handle the request?
2. Which instance of that service should handle it?

This is solved using a Load Balancer.

---

# Two Levels of Routing

### 1️⃣ Service-Level Routing

Handled by L7 Load Balancer

### 2️⃣ Instance-Level Distribution

Handled by Load Balancing Algorithm

---

# L4 vs L7 Load Balancer

## L4 Load Balancer (Transport Layer)

Works at Layer 4 of OSI.

Understands:

* TCP
* UDP

Does NOT understand:

* HTTP
* Headers
* Cookies
* URL paths

Routing decision is based only on:

* Source IP
* Source Port
* Destination IP
* Destination Port
* Protocol

Example:

* AWS Network Load Balancer

Best for:

* WebSocket
* Real-time chat
* High-performance TCP systems

L4 does not inspect request content.
It routes purely based on connection metadata.

---

## L7 Load Balancer (Application Layer)

Works at Layer 7.

Understands:

* HTTP
* HTTPS
* WebSocket (via HTTP upgrade)
* Headers
* Cookies
* URL paths
* Hostname

Examples:

* NGINX
* HAProxy
* AWS Application Load Balancer

L7 can perform:

* Path-based routing
* Header-based routing
* Cookie-based routing
* Host-based routing

---

# Example 1: Update Profile (Multiple Services)

User sends request:

PUT /user/update-profile

Flow:

1. Request hits L7 Load Balancer.
2. L7 reads the path `/user/*`.
3. It routes to UserService.
4. Load balancing algorithm selects one instance (e.g., UserService-2).
5. Request is forwarded.

Here:

* L7 decides the service.
* Algorithm decides the instance.

---

# Example 2: Chat with WebSocket

User connects using WebSocket.

Flow:

1. Connection hits Load Balancer.
2. L4 handles TCP connection.
3. It assigns connection to ChatService-1.
4. All future messages go to same instance until connection closes.

This ensures connection stickiness.

---

# Load Balancing Algorithms

There are two types:

1. Static Algorithms
2. Dynamic Algorithms

---

# Static Algorithms

Do NOT consider current server load.
Follow predefined rules.

---

## Round Robin

Requests are distributed sequentially.

Example with 3 servers:

Request 1 → Server A
Request 2 → Server B
Request 3 → Server C
Request 4 → Server A

Pros:

* Simple
* Even distribution (if servers equal)

Cons:

* Does not consider real-time load

Best for:

* Similar capacity servers

---

## Weighted Round Robin

Each server has a weight.

Example:

Server A (weight 3)
Server B (weight 1)

Traffic pattern:

A → A → A → B → repeat

Used when:

* Some servers are more powerful

---

## IP Hash

Routing based on client IP.

Same IP → same server.

Example:

* User A always routed to Server B
* User B always routed to Server C

Used for:

* Session stickiness
* Stateful services

Problem:

* Uneven traffic if many users behind same NAT

---

# Dynamic Algorithms

Consider real-time server condition.

---

## Least Connections

Request goes to server with fewest active connections.

Example:

Server A → 100 active connections
Server B → 40 active connections

Next request → Server B

Best for:

* WebSocket
* Long-lived connections

---

## Weighted Least Connections

Similar to Least Connections but considers server capacity.

More powerful server can handle more connections.

---

## Least Response Time

Request goes to server responding fastest.

Considers:

* Response latency
* Active connections

Best for:

* Latency-sensitive systems

---

# How Everything Works Together

1. L7 decides which service based on path/headers.
2. Load balancing algorithm selects best instance.
3. L4 maintains connection for TCP/WebSocket.
4. Business logic stays inside service.
5. Cross-instance communication handled via Redis/Kafka if required.

---

# Interview Summary

* L4 works at transport layer (TCP/UDP).
* L7 works at application layer (HTTP-aware).
* Static algorithms use fixed rules.
* Dynamic algorithms use runtime metrics.
* WebSocket chat commonly uses L4 + Least Connections.
* Multi-service routing requires L7.
* Load balancer never contains business logic.

---

Perfect 👌
Let’s extend your **Load Balancing Algorithms notes** properly with:

* ✅ What problem it solves
* ✅ Pros
* ❌ Cons
* ➡️ What next algorithm solves

Clean plain Markdown for your README.

---

# Load Balancing Algorithms (Detailed)

Load balancing algorithms are divided into:

1. Static Algorithms
2. Dynamic Algorithms

---

# 1️⃣ Static Algorithms

Static algorithms do NOT consider real-time server load.
They follow predefined distribution rules.

---

## 🔹 Round Robin

### How it works

Requests are distributed sequentially across servers.

Example:
Server A → Server B → Server C → repeat

---

### ✅ Solves

* Simple traffic distribution
* Basic horizontal scaling
* Prevents sending all traffic to one server

---

### ✅ Pros

* Very simple to implement
* Works well if all servers have equal capacity
* Low overhead

---

### ❌ Cons

* Does not consider server load
* Ignores CPU, memory, response time
* Can overload a slower server

---

### ➡️ Next Improvement

Weighted Round Robin solves the unequal server capacity problem.

---

## 🔹 Weighted Round Robin

### How it works

Each server has a weight based on its capacity.

Example:
Server A (weight 3)
Server B (weight 1)

Traffic pattern:
A → A → A → B → repeat

---

### ✅ Solves

* Unequal hardware capacity
* More powerful servers get more traffic

---

### ✅ Pros

* Better distribution than simple Round Robin
* Simple logic
* Useful in heterogeneous environments

---

### ❌ Cons

* Still does NOT consider real-time load
* If a server becomes slow temporarily, it still gets traffic
* Cannot adapt dynamically

---

### ➡️ Next Improvement

Least Connections solves real-time load imbalance.

---

## 🔹 IP Hash

### How it works

Server is selected based on client IP hash.

Same client IP → same server.

---

### ✅ Solves

* Session stickiness
* Stateful application routing

---

### ✅ Pros

* No need for external session store
* Ensures user consistency
* Good for chat or login sessions

---

### ❌ Cons

* Uneven traffic if many users behind same NAT
* Not ideal for mobile networks
* Does not consider server health

---

### ➡️ Next Improvement

Consistent Hashing improves uneven distribution issue.

---

# 2️⃣ Dynamic Algorithms

Dynamic algorithms consider real-time server metrics.

---

## 🔹 Least Connections

### How it works

Request goes to the server with the fewest active connections.

---

### ✅ Solves

* Uneven load distribution
* Long-lived connections (like WebSocket)
* Real-time traffic balancing

---

### ✅ Pros

* Adapts to server load
* Ideal for chat systems
* Prevents overloaded servers

---

### ❌ Cons

* Slightly more complex
* Requires tracking active connections
* Does not consider response latency

---

### ➡️ Next Improvement

Least Response Time solves latency issue.

---

## 🔹 Weighted Least Connections

### How it works

Combination of server capacity + active connections.

More powerful servers can handle more connections.

---

### ✅ Solves

* Capacity difference
* Real-time load imbalance

---

### ✅ Pros

* Better than simple Least Connections
* Adapts dynamically
* Good for large-scale systems

---

### ❌ Cons

* More complex to configure
* Requires monitoring metrics

---

### ➡️ Next Improvement

Least Response Time considers latency as well.

---

## 🔹 Least Response Time

### How it works

Routes request to the server with:

* Lowest response time
* Lowest active connections

---

### ✅ Solves

* Slow server detection
* Latency-sensitive systems
* Performance bottlenecks

---

### ✅ Pros

* Best for high-performance APIs
* Automatically avoids slow servers
* Improves user experience

---

### ❌ Cons

* Requires constant monitoring
* Higher computational overhead
* More complex implementation

---

# Interview-Level Understanding

Static algorithms:

* Simple
* Low overhead
* Not adaptive

Dynamic algorithms:

* Load-aware
* More intelligent
* Slightly more complex

---

# Real-World Recommendation

For Chat Systems:

* L4 + Least Connections

For REST APIs:

* L7 + Least Response Time

For Mixed Systems:

* L7 for routing
* Dynamic algorithm for instance selection

---
