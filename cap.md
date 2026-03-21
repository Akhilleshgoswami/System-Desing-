# CAP Theorem (Consistency, Availability, Partition Tolerance)

CAP Theorem is a fundamental concept in **Distributed Systems**.

It states that a distributed system can guarantee only **two out of three** properties at the same time:

- Consistency (C)  
- Availability (A)  
- Partition Tolerance (P)  

---

# What is CAP Theorem?

In a distributed system (like master-slave databases), data is stored across multiple nodes.

CAP defines the behavior of the system when there is a **network failure (partition)**.

---

# 1️⃣ Consistency (C)

## Definition

All nodes should have the **same data at the same time**.

After a write operation:
- Every read should return the **latest updated value**

---

## Example

- Value of `A = 10`  
- User updates it to `A = 5`  

Now:
- DB Node B → A = 5  
- DB Node C → A = 5  

👉 Both nodes have same data → Consistent system  

---

# 2️⃣ Availability (A)

## Definition

Every request should receive a response:
- Either **success** or **failure**
- But system should NOT hang

---

## Example

- User sends request to read data  
- Even if one node fails, another node should respond  

👉 System is always available  

---

# 3️⃣ Partition Tolerance (P)

## Definition

System continues to work even if there is a **network failure between nodes**.

---

## Example

- DB Node B and DB Node C cannot communicate  
- Data cannot replicate between them  

👉 This situation is called a **network partition**

System should still:
- Accept requests  
- Continue running  

---

# Understanding with Example

## Setup

- Application Server  
- Two DB Nodes (B and C)

---

## Case 1: Consistency + Availability (No Partition)

- A = 10  
- User updates A → 5  
- Data replicated to both nodes  

Now:
- Node B → 5  
- Node C → 5  

✔ Consistency achieved  
✔ Both nodes respond → Availability achieved  

---

## Case 2: Add Partition Tolerance

Now suppose:

- Network breaks between Node B and Node C  

This means:
- Nodes cannot sync data  

---

## Problem

User updates value:

- Node B → A = 5  
- Node C → A = 10 (old value, not updated)

Now:

### If we choose Consistency (C)

- We must block reads/writes until nodes sync  
- Some requests will fail  

❌ Availability is lost  

---

### If we choose Availability (A)

- Both nodes will respond  
- But data will be different  

❌ Consistency is lost  

---

# Final Conclusion

👉 In presence of **Partition (P)**, we must choose:

- Either **Consistency (C)**  
- Or **Availability (A)**  

We cannot have all three together.

---

# Real-World Systems

## CP Systems (Consistency + Partition Tolerance)
- Prefer correct data over availability  
- Example: Banking systems  

## AP Systems (Availability + Partition Tolerance)
- Prefer system availability over consistency  
- Example: Social media feeds  

---

# Summary

- Consistency → Same data across all nodes  
- Availability → Every request gets a response  
- Partition Tolerance → System works despite network failure  

👉 CAP Theorem says:  
**You can only choose 2 out of 3 in a distributed system**

---

💡 Key Insight:
When network partition happens, system designers must decide:
- Do we want correct data? (Consistency)  
- Or always-on system? (Availability)  
