
# How Node.js Handles Requests

Node.js uses an **event-driven, non-blocking architecture** to handle requests efficiently. Let’s break it down step by step.

---

## 1️⃣ Client → Server

When a client sends a request:

- Request arrives at the Node.js server
- It first goes into the **Event Queue**

---

## 2️⃣ Event Loop

- The **Event Loop** constantly monitors the Event Queue
- It picks requests from the queue one by one
- Determines whether the request is:

  1. **Blocking / Synchronous** (e.g., CPU-intensive task, file system sync read)  
  2. **Non-blocking / Asynchronous** (e.g., HTTP requests, DB queries, timers)

---

## 3️⃣ Processing Requests

### Non-Blocking / Async Tasks

- Handled directly by the Event Loop
- Processed immediately without waiting
- Response sent to the client once task is done

### Blocking / Sync Tasks

- Sent to **Thread Pool** (provided by libuv)
- Thread Pool has limited threads (default: **4**)  
- Each thread works on a blocking task independently
- Once task completes → result returned to Event Loop → response sent to client

---

## 4️⃣ Thread Pool

- Handles all **blocking operations** (file I/O, crypto, DB operations)  
- Default **4 threads**  
- Can be increased in Node.js using environment variable in your terminal before starting app:

On Linux / Mac:

export UV_THREADPOOL_SIZE=8  

On Windows (PowerShell):

$env:UV_THREADPOOL_SIZE=8  

- If all threads are busy, incoming blocking requests wait until a thread is free → potential scalability bottleneck

---

## 5️⃣ Synchronous vs Asynchronous

| Type        | Behavior                          | Event Loop | Thread Pool |
|------------|----------------------------------|------------|------------|
| Sync / Blocking | Stops event loop until complete | ❌          | ✅ (thread handles) |
| Async / Non-blocking | Does not block event loop | ✅          | ❌           |

**Rule of thumb:**  
- All synchronous operations **block** Node.js  
- All asynchronous operations **do not block**

---

## 6️⃣ Scaling Considerations

- Default thread pool = 4 → may be too low for CPU-intensive operations  
- Thread pool can be increased → consider number of CPU cores  
- Use **async operations** wherever possible to maximize concurrency

---

## 7️⃣ Getting System Info

- Node.js **os module** can provide info about CPU cores, memory, etc.

Example:

- `os.cpus()` → number of CPU cores  
- `os.totalmem()` → total memory  
- Helps decide **optimal thread pool size**

---

## Summary

1. Client request → Event Queue  
2. Event Loop picks request → identifies blocking vs non-blocking  
3. Non-blocking → processed directly  
4. Blocking → sent to Thread Pool  
5. Thread Pool processes tasks → Event Loop sends response  
6. Default threads = 4, configurable based on CPU cores  
7. Always prefer async operations for better scalability
