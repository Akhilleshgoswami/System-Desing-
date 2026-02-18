
---

# Monolithic Architecture

## Definition

A **Monolithic Architecture** is a system design where:

* All modules are part of a **single codebase**
* Built together
* Tested together
* Run together
* Deployed as a single unit

All components (Order, Payment, User, Inventory, etc.) are **tightly coupled** and usually share the same database.

---

# Core Characteristics

* Single deployable application
* Single runtime process
* Shared database
* Tight coupling between modules
* One failure can affect the entire system

---

# Problems with Monolithic Architecture

## 1. Single Point of Failure (SPOF)

Since everything runs in one application:

If **any critical module fails**, the **entire system can crash**.

### Example: Order & Payment Module

System contains:

* Order Module
* Payment Module

Flow:

1. User places an order
2. Order module calls Payment module
3. Payment processes transaction

If:

* Payment service throws an unhandled exception
* Payment gateway times out
* A bug exists in payment logic

Then:

* The whole application may crash
* Users cannot place orders
* Users may not even browse products

👉 A failure in one module brings down the entire system.
This is called a **Single Point of Failure**.

---

## 2. Deployment Bottleneck

In monolithic systems:

Even a small change requires redeploying the entire application.

### Example:

You want to add **Credit Card support** in the Payment module.

Only Payment logic changes.

But because everything is tightly coupled:

* Full application must be rebuilt
* Full regression testing required
* Entire system must be redeployed

Issues:

* Slower release cycles
* Higher deployment risk
* Unnecessary downtime

---

## 3. No Independent Scaling

Scaling happens at the application level — not module level.

### Example:

If:

* Payment module has heavy traffic (e.g., sale day)
* Order module has normal traffic

You cannot scale Payment alone.

Instead:

* You must scale the entire application

Result:

* Resource wastage
* Higher infrastructure cost
* Inefficient scaling

---

## 4. Tight Coupling

Modules are strongly dependent on each other:

* Direct method calls
* Shared database tables
* Shared business logic

Impact:

* Hard to modify or refactor
* Small changes can affect other modules
* Maintenance becomes difficult as system grows

---

# Summary

A **Monolithic Architecture** is simple to start with but becomes difficult to scale and maintain as the system grows.

Main drawbacks:

* Single Point of Failure
* Deployment Bottleneck
* No Independent Scaling
* Tight Coupling
* Reduced flexibility

---

# Interview Explanation (2–3 Minutes)

A monolithic architecture is a system where the entire application is built and deployed as a single unit. All modules such as Order, Payment, and User are tightly coupled and run inside the same process, usually sharing the same database.

This approach works well for small systems because it is simple to develop and deploy.

However, it has major limitations:

First, it creates a single point of failure. If the Payment module crashes, the entire application can go down.

Second, it causes deployment bottlenecks. Even a small change in one module requires redeploying the whole system.

Third, there is no independent scaling. If only Payment needs more resources, we still have to scale the entire application.

So while monolithic architecture is good for early-stage applications, it becomes difficult to maintain and scale as the system grows. That’s why larger systems often move to microservices.

---


---

# Microservices Architecture

## Definition

A **Microservices Architecture** is a system design where:

* The application is divided into **small, independent services**
* Each service handles a **single business capability**
* Services are developed, tested, deployed, and scaled **independently**

Each service runs in its own process and communicates with other services using APIs (usually HTTP/REST or messaging queues).

---

# Example: Order & Payment Services

Instead of one big application, we create:

* Order Service
* Payment Service
* User Service
* Inventory Service

Each service:

* Has its own codebase
* Can have its own database
* Runs independently
* Is deployed independently

So:

* Order service runs separately
* Payment service runs separately
* If needed, we update only one service without affecting others

---

# Core Characteristics

* Independent services
* Independent deployment
* Independent scaling
* Loose coupling
* Often database per service
* Communication via APIs or events

---

# Advantages of Microservices

## 1. No Single Point of Failure (Better Isolation)

If the Payment service crashes:

* Order service can still run
* Users may still browse products
* Other services are not directly affected

Failure is isolated to one service.

This improves system resilience.

---

## 2. Independent Deployment

If we want to add a **Credit Card feature** in Payment service:

* Only Payment service is modified
* Only Payment service is tested
* Only Payment service is deployed

No need to redeploy the entire system.

This allows:

* Faster release cycles
* Smaller deployment risk
* Continuous delivery

---

## 3. Independent Scaling

If Payment service receives heavy traffic during a sale:

* We scale only the Payment service
* No need to scale Order or User services

This leads to:

* Efficient resource usage
* Lower infrastructure cost
* Better performance optimization

---

## 4. Loose Coupling

Services communicate via APIs instead of direct function calls.

This makes:

* Refactoring easier
* Codebase cleaner
* Teams work independently
* Technology flexibility (one service can use Node.js, another Go, etc.)

---

# Challenges of Microservices

Microservices also introduce complexity:

* Network latency (service-to-service calls)
* Distributed transactions are harder
* Data consistency challenges
* Monitoring and observability become important
* DevOps and infrastructure complexity increases

So it is more powerful but also more complex.

---

# Summary

A **Microservices Architecture** splits the application into small, independent services that can be built, deployed, scaled, and maintained separately.

Main benefits:

* Fault isolation
* Independent deployment
* Independent scaling
* Loose coupling
* Faster development cycles

However, it increases operational and architectural complexity.

---

# Interview Explanation (2–3 Minutes)

Microservices architecture is a design pattern where we break a large application into small, independent services based on business capabilities.

For example, instead of one big application containing Order and Payment modules, we create separate services — an Order service and a Payment service.

Each service runs independently, has its own codebase, and can be deployed and scaled separately.

The major advantage is fault isolation. If the Payment service fails, the Order service and other services can still function. This reduces the risk of a complete system failure.

Second, we get independent deployment. If we add a credit card feature to the Payment service, we only deploy that service — not the entire system.

Third, we get independent scaling. If Payment receives heavy traffic, we scale only Payment instead of the whole application.

However, microservices introduce complexity like network communication, distributed transactions, and infrastructure management.

So microservices are powerful for large-scale systems but require mature engineering practices to manage properly.

---

# Migration from Monolith to Microservices

## Important Principle

Migrating from monolith to microservices is **not a one-day activity**.

* We cannot migrate the whole system at once.
* Migration must be done **incrementally**.
* We move **one module at a time**.

This is usually done using the **Strangler Pattern** (gradually replacing parts of the monolith).

---

# Step 1: Understand Your Monolith

Before migration, we must clearly understand:

* What modules exist?

  * Auth Module
  * Search Module
  * Order Module
  * Cart Module
  * Payment Module
  * Inventory Module
* How they interact
* Shared database dependencies
* Tight coupling areas
* Traffic patterns

Example (like Amazon-type system):

All modules are inside one large application.

---

# Step 2: Identify High-Impact Areas

We do not randomly pick modules.

We identify:

* High traffic modules
* High failure risk modules
* Frequently changing modules
* Performance bottlenecks

### Example:

Suppose:

* Payment module handles transactions
* It integrates with external gateways
* It has high traffic during sales
* A crash here could affect the entire system

This becomes a **high-impact area**.

So we choose Payment as the first candidate to extract.

---

# Step 3: Extract First Microservice (Example: Payment Service)

We:

* Create a new independent Payment service
* Move payment-related logic from monolith into this service
* Give it its own database (if possible)

Now:

* Order module in monolith no longer calls internal payment function
* Instead, it calls Payment service via API

---

# Step 4: Define API Contracts & Communication

We must clearly define:

## 1. API Contracts

* Request/response structure
* Status codes
* Error handling
* Authentication between services

Example:
Order Service → Payment Service
`POST /payments`

---

## 2. Communication Type

We decide:

### Synchronous Communication

* HTTP / REST
* Used when immediate response is required
* Example: Payment confirmation

### Asynchronous Communication

* Message queues (Kafka, RabbitMQ, SQS)
* Used for event-driven workflows
* Example: “Payment Completed” event

We carefully choose:

* Which communication style?
* Which message queue?
* Retry strategy?
* Timeout handling?

---

# Step 5: Gradual Migration (Module by Module)

After Payment:

* Extract Order
* Extract Cart
* Extract Inventory

We do not break everything at once.

We gradually reduce responsibilities of monolith until it becomes smaller and eventually removed.

This approach reduces risk.

---

# Step 6: Testing & Monitoring

Migration is incomplete without:

## Testing

* Unit testing
* Integration testing
* Contract testing
* End-to-end testing

We must ensure:

* No data inconsistency
* No broken workflows
* No unexpected failures

---

## Monitoring & Observability

Since microservices are distributed:

We need:

* Logging
* Metrics
* Distributed tracing
* Health checks

We monitor:

* Latency
* Error rates
* Service downtime
* Queue lag (if using messaging)

---

# Key Challenges During Migration

* Data migration complexity
* Distributed transactions
* Increased network latency
* Service discovery setup
* DevOps maturity required

Migration increases operational complexity, so it must be done carefully.

---

# Summary

Migrating from monolith to microservices:

* Is incremental
* Requires deep understanding of the monolith
* Starts with high-impact modules
* Requires API contracts & communication design
* Needs strong testing and monitoring
* Must be done module by module

It is a strategic engineering decision — not just code refactoring.

---

# Interview Explanation (2–3 Minutes)

Migrating from monolith to microservices is not something we do in one go. It’s an incremental process.

First, we deeply understand the monolith — identify modules like Auth, Search, Order, Cart, and Payment, and understand their dependencies.

Second, we identify high-impact areas. For example, if the Payment module handles high traffic and is prone to failure, we extract it first.

We then create an independent Payment microservice and define clear API contracts. The monolith now communicates with the Payment service using synchronous REST APIs or asynchronous messaging depending on the use case.

After successful extraction, we gradually migrate other modules one by one.

Throughout this process, testing and monitoring are critical because distributed systems introduce complexity like network failures and data consistency issues.

So migration is a gradual, carefully planned strategy rather than a one-time rewrite.

---


