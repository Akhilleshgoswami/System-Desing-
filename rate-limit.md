
# Rate Limiting in System Design

## What is Rate Limiting?

Rate limiting is a technique used to **control how many requests a user can send to a server in a given time**.

---

## Why Do We Need Rate Limiting?

Imagine:

- You have an application with multiple users  
- All users send requests to the server  

Problem:

- Some users (or bots) send **too many requests**  
- Server resources get exhausted  
- Genuine users face delays or failures  

---

## Example

User A sends many repeated requests:

- Server becomes overloaded  
- Genuine users cannot get proper response  

### Solution: Rate Limiting

- Limit how many requests a user can make  
- Example: **3 requests per second per user/IP**

If user exceeds limit:
- Server rejects request  
- Returns **HTTP 429 (Too Many Requests)**  

---

## How Rate Limiting Works

- Requests are tracked using:
  - IP Address  
  - User ID  
  - API Key  

- If request limit exceeded:
  - Request is blocked temporarily  
  - User must wait for next time window  

---

# 1️⃣ Token Bucket Algorithm

## Concept

- We have a **bucket filled with tokens**  
- Each request needs **1 token**  
- If token available → request allowed  
- If no token → request rejected  

---

## How It Works

- Bucket capacity = 5 tokens  
- Refill rate = 3 tokens per second  

### Example

- User A, B, C send requests → each takes 1 token  
- Tokens reduce from 5 → 2  

Now:

- User A sends 3 more requests quickly  
- All tokens consumed → bucket empty  

Next request:

- User D sends request → ❌ rejected (no token)  

After 1 second:
- Bucket refills (3 tokens added)  
- Requests can be processed again  

---

## Advantages

- Allows burst traffic  
- Smooth request handling  
- Flexible rate control  

---

## Drawbacks

- One user can consume all tokens quickly  
- Other users may get blocked  
- Not fair in high concurrency  

---

# 2️⃣ Leaky Bucket Algorithm

## Concept

- Requests go into a **bucket (queue)**  
- Requests are processed at a **fixed rate**  
- Extra requests are dropped  

---

## How It Works

- Bucket processes requests at fixed speed (e.g., 2 req/sec)  

### Example

- 5 users send requests at same time  
- Bucket allows only 2 requests per second  
- Remaining requests:
  - Either queued  
  - Or dropped if bucket is full  

---

## Advantages

- Smooth and constant request rate  
- Prevents sudden spikes  
- More predictable system behavior  

---

## Drawbacks

- Does not allow bursts  
- Requests may be delayed  
- Can drop valid requests during peak  

---

# Token Bucket vs Leaky Bucket

| Feature | Token Bucket | Leaky Bucket |
|--------|-------------|-------------|
| Burst traffic | Allowed | Not allowed |
| Flexibility | High | Low |
| Request handling | Based on tokens | Fixed rate |
| Fairness | Less fair | More controlled |

---

# Summary

- Rate limiting protects server from overload  
- Ensures fair usage for all users  
- Returns HTTP 429 when limit exceeded  

## Algorithms

- Token Bucket → allows bursts, flexible  
- Leaky Bucket → smooth, constant rate  

---

💡 Key Insight:

Rate limiting is essential for:
- Preventing abuse  
- Protecting system resources  
- Ensuring good experience for genuine users  

