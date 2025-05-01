
---

# Distributed Systems: CAP Theorem and the PACELC Model

---

## 1. CAP Theorem

### Origin and Context
The **CAP theorem**, proposed by **Eric Brewer** and formally proven by Gilbert and Lynch, applies specifically to **distributed systems**, not to single-node environments. It becomes relevant when a system is deployed across **multiple nodes** or **data centers** that may experience **network partitions**.

### Definition
The **CAP theorem** states that in the presence of a **network partition**, a distributed system can only **guarantee two out of the following three properties**:

1. **Consistency (C)**
2. **Availability (A)**
3. **Partition Tolerance (P)**

---

## 2. Definitions of CAP Properties

### **Partition Tolerance (P)**
- The system continues to operate correctly even if communication between nodes is **partially or entirely interrupted**.
- This models real-world situations such as **network failures, latency spikes, or dropped packets**.
- In **distributed environments**, **partition tolerance is always required**, since network failures are inevitable.

### **Consistency (C)**
- Every **read** receives the **most recent write** for a given piece of data.
- Equivalent to **linearizability**: operations appear to happen in a **single, serial order**.
- No stale or divergent data is ever served.

### **Availability (A)**
- Every **request** (read or write) receives a **non-error response**, even if it may not reflect the most recent write.
- The system must always remain responsive.

---

## 3. The CAP Trade-off Explained

Since **network partitions are unavoidable**, the real-world trade-off is typically between:

- **Consistency vs. Availability**, in the presence of **Partition Tolerance (P)**.

### Why Can't We Have All Three?

Consider a scenario where a network partition occurs:
- Two nodes cannot communicate with each other.
- Now, suppose a client sends a **read or write** to each partitioned node.

At this point:
- If the system chooses **Consistency**, one partition must **refuse to respond** (breaking Availability).
- If it chooses **Availability**, both partitions will respond but may return **divergent or stale data** (breaking Consistency).

Thus, in the presence of a partition, you must choose:
- **CP**: Sacrifice **Availability** to preserve **Consistency**
- **AP**: Sacrifice **Consistency** to preserve **Availability**

**CA** is theoretically possible only in **non-partitioned environments** (e.g., single-node or fully synchronous networks), but this is **not realistic** in most practical distributed systems.

---

## 4. CAP Examples in Practice

### **CP (Consistent and Partition Tolerant)**
- Systems like **HBase**, **MongoDB (with strong consistency mode)**, and **Zookeeper**.
- These systems **refuse operations** when partitions occur to ensure data consistency.

### **AP (Available and Partition Tolerant)**
- Systems like **Cassandra**, **DynamoDB**, and **Riak**.
- These systems continue to serve **read/write operations**, even during partitions, which may cause **eventual consistency**.

---

## 5. PACELC Model: Extending CAP

The **PACELC model**, proposed by **Daniel Abadi**, extends CAP by incorporating **latency and consistency trade-offs**, even **when there is no partition**.

### PACELC Breakdown

- **If Partition occurs (P)**, the system must choose between:
  - **A** (Availability)
  - **C** (Consistency)
- **Else (E)**, when there is **no partition**, the system must choose between:
  - **L** (Latency)
  - **C** (Consistency)

### Complete Form
**PACELC** stands for:

> **P**artition → **A**vailability or **C**onsistency,  
> **E**lse → **L**atency or **C**onsistency

This model shows that even without partitions, designers still have to **trade off latency vs consistency**.

---

## 6. PACELC Examples

| System      | Partition Tradeoff | Else (No Partition) Tradeoff |
|-------------|---------------------|-------------------------------|
| **Cassandra**   | **AP**               | **EL** (Low Latency)             |
| **DynamoDB**    | **AP**               | **EL**                           |
| **HBase**       | **CP**               | **EC** (Consistency over latency)|
| **Spanner (Google)** | **CP**          | **EC**                           |
| **MongoDB** (eventual consistency) | **AP** | **EL**                    |

---

## 7. Why PACELC Matters

The **CAP theorem** only models system behavior **during partitions**.  
**PACELC** gives a more **complete picture**, as in real-world deployments:

- Partitions are **infrequent**, but **latency** is an **ever-present concern**.
- High availability systems often prefer **low latency** over consistency (e.g., caching systems).
- Critical systems (e.g., finance) often prioritize **strong consistency**, even at the cost of latency.

---
