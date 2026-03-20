# Distributed System

## Definition

A **Distributed System** is a system where multiple independent computers (nodes) work together to solve a problem.

Even though multiple machines are involved, the system appears as a **single system** to the user.

- Multiple computers  
- Working together  
- Appears as one system  

---

## Simple Understanding

Instead of running everything on one server, we use multiple servers.

Each server:
- Works independently  
- Handles requests  
- Coordinates with other servers  

But from the user's perspective:

👉 It feels like a single system is handling everything  

---

## Example: E-commerce Application

Suppose you have an e-commerce application.

Instead of hosting it on one server, you deploy it on:

- Server 1  
- Server 2  
- Server 3  

Now:
- All servers handle user requests  
- Load balancer distributes traffic  
- Each server performs the same work  

If one server fails:
- Other servers continue working  

User still sees the application running normally.

---

## Relation with System Design Concepts

Distributed systems include:

- Load Balancing  
- Horizontal Scaling  
- Multiple Regions  
- Availability Zones  

All these concepts together form a distributed system.

---

## Advantages of Distributed Systems

### 1. Scalability

We can increase system capacity by adding more servers.

Example:
- More users → Add more machines  
- Traffic gets distributed  

---

### 2. Fault Tolerance

If one machine fails:
- Other machines continue working  
- System does not completely go down  

Example:
- Server 1 crashes → Server 2 & Server 3 handle requests  

---

### 3. Low Latency

We can place servers closer to users.

Example:
- Users in India → Served from India region  
- Users in Europe → Served from Europe region  

This reduces response time.

---

### 4. High Availability

System remains available even during failures.

- Multiple servers ensure uptime  
- Requests are always handled  

Example:
- One data center fails → Traffic is routed to another  

---

### 5. Handling Large Scale Systems

Distributed systems can handle:

- Millions of users  
- Large data processing  
- High traffic applications  

Example:
- Amazon, Netflix, Google  

---

## Challenges

- Network communication delays  
- Data consistency issues  
- Complex architecture  
- Debugging is harder  
- Distributed failures  

---

## Summary

A **Distributed System** is a group of independent computers working together as a single system.

It provides:

- Scalability  
- Fault tolerance  
- Low latency  
- High availability  

It is the foundation of modern large-scale applications.

---

## Interview Explanation (2–3 Minutes)

A distributed system is a system where multiple independent machines work together, but to the user it appears as a single system.

For example, in an e-commerce application, instead of running everything on one server, we deploy it across multiple servers. A load balancer distributes requests among them, and all servers perform the same tasks.

The main advantages are scalability, fault tolerance, low latency, and high availability. We can scale by adding more machines, and if one machine fails, others continue working.

Distributed systems also allow us to deploy across regions to reduce latency.

However, they introduce challenges like network delays, data consistency issues, and increased complexity.

Overall, distributed systems are essential for building scalable and reliable applications.
