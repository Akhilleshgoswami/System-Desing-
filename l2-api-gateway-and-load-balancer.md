---

# API Gateway

## Definition

An API Gateway is a single entry point for all client requests in a microservices architecture.

Instead of the client calling multiple microservices directly, the client sends all requests to the API Gateway. The gateway then forwards the request to the appropriate microservice.

The client never communicates with microservices directly.

---

# Why Do We Need API Gateway?

In a microservices architecture, we may have:

* User Service
* Order Service
* Payment Service
* Cart Service

Without an API Gateway:

* The client must know the URL of every service
* The client must handle authentication
* The client must make multiple network calls
* The system becomes complex and tightly coupled to service structure

The API Gateway simplifies this by acting as a central access point.

---

# Example (Amazon-like System)

Suppose we have:

* User Service
* Order Service
* Payment Service
* Cart Service

### Scenario: User Updates Name

A user wants to update their username.

From the user's perspective:

* They open the UI
* Click on Update Profile
* Submit the new name

The user does not know anything about backend endpoints.

Actual flow:

1. Frontend sends request to API Gateway
2. API Gateway forwards request to User Service
3. User Service updates the database
4. Response goes back through API Gateway to client

Flow becomes:

Client → API Gateway → User Service

This hides internal service structure from the client.

---

# Core Responsibilities of API Gateway

## 1. Request Routing

The gateway routes incoming requests to the correct microservice.

Example:

* Order-related requests → Order Service
* Payment-related requests → Payment Service
* Cart-related requests → Cart Service

---

## 2. Authentication and Authorization

Instead of implementing authentication in every microservice, we centralize it at the API Gateway.

The gateway can:

* Validate JWT tokens
* Verify user identity
* Check role-based permissions
* Reject unauthorized requests

This improves security and consistency.

---

## 3. API Composition

Sometimes a client needs data from multiple services.

Example:

A mobile home screen may need:

* User details
* Recent orders
* Cart summary

Instead of making three separate calls from the client:

The API Gateway calls multiple services internally, combines the responses, and returns a single response.

This is called API Composition.



# Advantages of API Gateway

* Centralized request handling
* Improved security
* Simplified client communication
* API aggregation (composition)
* Better monitoring and logging
* API version management

---

# Summary

An API Gateway acts as the single entry point in a microservices architecture.

It handles:

* Routing
* Authentication and Authorization
* API Composition
* Device-specific responses
* Integration with service discovery

It simplifies the system and improves security, but it must be highly available and scalable.

---

# Interview Explanation (2–3 Minutes)

In microservices architecture, instead of letting the client directly communicate with multiple services, we introduce an API Gateway as a single entry point.

For example, in an Amazon-like system with User, Order, Payment, and Cart services, when a user updates their profile, the request first goes to the API Gateway. The gateway then forwards it to the User Service.

The API Gateway handles request routing, authentication, authorization, and sometimes combines responses from multiple services, which is known as API composition.

It also helps in device-specific responses and integrates with service discovery to route traffic dynamically.

Overall, API Gateway simplifies client-side logic and centralizes cross-cutting concerns like security and monitoring, making the architecture cleaner and more manageable.




---

# Load Balancer

## Definition

A **Load Balancer** is a component that distributes incoming requests across multiple instances of a service.

It ensures that traffic is evenly distributed so that:

* No single instance is overloaded
* System performance remains stable
* High availability is maintained

In simple terms, a Load Balancer decides **which service instance should handle the request**.

---

# Why Do We Need a Load Balancer?

In microservices architecture:

* Each microservice can have multiple instances
* Services scale horizontally (multiple copies running)

If we don’t use a Load Balancer:

* One instance may receive all traffic
* Other instances may remain idle
* System may crash due to overload

Load Balancer solves this problem.

---

# Example (Payment Microservice)

Suppose:

We have a Payment microservice.

To handle high traffic, we run:

* Payment Instance 1
* Payment Instance 2

Now:

A user wants to make a payment.

Flow:

1. Client sends request to API Gateway
2. API Gateway forwards request to Payment service
3. Load Balancer decides which Payment instance should handle it

Possible situation:

* Instance 1 is busy
* Instance 2 is free

The Load Balancer routes the request to Instance 2.

So instead of the client choosing the instance, the Load Balancer automatically routes it.

In simple words:

Load Balancer = Smart router for service instances.

---

# What Does a Load Balancer Do?

* Distributes traffic across instances
* Prevents overload on a single instance
* Improves performance
* Improves availability
* Helps with horizontal scaling

---

# Where Does Load Balancer Sit?

In a microservices architecture:

Client → API Gateway → Load Balancer → Service Instances

Or sometimes:

Client → Load Balancer → API Gateway → Services

It depends on system design.

---

# Advantages

* Better resource utilization
* High availability
* Fault tolerance
* Improved scalability
* Prevents system overload



# Summary

A Load Balancer distributes traffic across multiple service instances.

It ensures:

* Even traffic distribution
* Better performance
* High availability
* Horizontal scalability

It acts like a smart router that decides which instance should handle the request.

---

# Interview Explanation (2–3 Minutes)

In microservices architecture, we often run multiple instances of the same service to handle high traffic. For example, the Payment service may have two or more running instances.

When a user makes a payment, the request first goes through the API Gateway. Then, the Load Balancer decides which Payment instance should handle the request.

If one instance is busy and another is free, the Load Balancer routes the request to the free instance. This prevents overload and ensures better performance.

Load Balancers typically use strategies like round robin, least connections, or health-based routing.

So in simple terms, a Load Balancer is a smart router that distributes traffic evenly across service instances to ensure high availability and scalability.

---
Perfect bro ✅


Topic:
**Which comes first — API Gateway or Load Balancer?**
And how do Regions & Availability Zones work?

---

# API Gateway vs Load Balancer – Which Comes First?

## Short Answer (Interview)

It depends on the architecture, but typically:

Client → Load Balancer → API Gateway → Services

OR

Client → API Gateway → Load Balancer → Service Instances

Both are valid depending on system design.

In large-scale systems, we usually have **multiple layers of load balancing**.

---

# Global Architecture (Multi-Region Setup)

Let’s take a large-scale example like Amazon.

We have:

* Multiple Regions (e.g., Boston, London)
* Inside each Region → Multiple Availability Zones
* Inside each AZ → Multiple service instances

---

# Key Concepts

## 1. Region

A Region is a geographical area.

Example:

* US East (Boston)
* Europe (London)

Each region contains multiple data centers.

---

## 2. Availability Zone (AZ)

Inside a Region, we have multiple Availability Zones.

Each AZ:

* Is a separate data center
* Has independent power & networking
* Can fail independently

Example:

Region: Boston

* AZ-1
* AZ-2
* AZ-3

---

# How Traffic Flows in Multi-Region Architecture

Step 1: User makes request
User calls: api.myapp.com

Step 2: DNS Load Balancer (Global Level)
Tools like:

* AWS Route 53
* Azure Traffic Manager

DNS decides:

Which Region should handle the request?

Example:

* If user is in US → Route to Boston
* If user is in Europe → Route to London

This is **Global Load Balancing**.

---

# Inside the Region

Once traffic reaches Boston region:

Now we have:

* API Gateway instances
* Multiple Availability Zones

Then:

Regional Load Balancer distributes traffic across AZs.

If:

* AZ-1 is healthy → Send traffic
* AZ-2 is down → Do NOT send traffic

So if one Availability Zone goes down:

Traffic is automatically routed to another healthy AZ.

This ensures high availability.

---

# Where Does API Gateway Sit?

Typical flow in large systems:

Client
→ Global DNS Load Balancer
→ Regional Load Balancer
→ API Gateway
→ Internal Load Balancer
→ Microservice Instances

So:

Load balancing happens at multiple levels.

---

# Failure Scenario Example

Let’s say:

Boston Region has:

* AZ-1
* AZ-2

If AZ-1 crashes:

* Load balancer stops sending traffic to AZ-1
* Traffic automatically shifts to AZ-2

If the entire Boston region fails:

* DNS routes traffic to London region

This ensures disaster recovery and high availability.

---

# Interview Explanation (2–3 Minutes)

In a microservices architecture deployed across multiple regions, traffic routing happens at multiple levels.

First, the client request hits a DNS-based global load balancer like Route 53. It decides which region should handle the request based on latency or geography.

Inside the selected region, a regional load balancer distributes traffic across multiple availability zones.

If one availability zone goes down, traffic is automatically routed to another healthy zone.

Within the region, the API Gateway acts as the single entry point to microservices. After the gateway, internal load balancers distribute traffic across service instances.

So both API Gateway and Load Balancer are used together, but load balancing happens at multiple layers — global level, regional level, and service level — to ensure high availability and fault tolerance.

---

# Important Interview Point

When interviewer asks:

“Which comes first — API Gateway or Load Balancer?”

Best answer:

In real-world distributed systems, we use multiple layers of load balancers. Typically, a global load balancer comes before the API Gateway, and internal load balancers exist after it to distribute traffic across service instances.

---

