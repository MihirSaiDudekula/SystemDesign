
---

# Distributed Database Systems: Replication and Sharding

---

## 1. Replication in Distributed Systems

### Definition
**Replication** is the process of **storing copies of the same data on multiple machines** (nodes) in a distributed system. It increases **availability**, **fault tolerance**, and **read scalability**.

### Core Motivation
- To ensure **high availability**: If one node fails, another can serve the data.
- To improve **read throughput** by distributing the load across replicas.
- To provide **redundancy** for disaster recovery and backups.

---

## 2. Leader-Follower Replication (also called Master-Slave Replication)

### Architecture
- One node is designated as the **Leader** (or **Master**) and handles all **writes**.
- One or more **Followers** (or **Slaves**) replicate the data from the leader and can serve **read** requests.

### Properties
- **Read/Write Permissions**:
  - **Leader**: Handles both **reads** and **writes**.
  - **Follower**: Handles only **reads**.
- Changes made to the leader are propagated to followers.

### Advantages
- **Read scalability**: Followers can serve read requests.
- **Simpler consistency** model: Since writes are centralized, consistency is easier to manage.

### Limitations
- **Write bottleneck**: All writes go through a single node.
- **Follower lag**: Followers may not have the latest data due to replication delay.

---

## 3. Synchronous vs Asynchronous Replication

### Synchronous Replication
- The leader waits for **acknowledgment from all followers** before considering a write successful.
- **Stronger consistency**, but slower writes.

#### Pros:
- Data is **always consistent** across replicas.
- Useful for **critical applications** like banking.

#### Cons:
- Slower due to waiting on network I/O.
- If a follower is slow or down, it can **delay the entire system**.

### Asynchronous Replication
- The leader **does not wait** for follower acknowledgments before responding to the client.
- Write is considered successful **immediately after committing on the leader**.

#### Pros:
- **Faster writes**, better performance.
- **Higher availability**; failure of follower doesn't block writes.

#### Cons:
- Followers may be **out-of-sync**.
- Risk of **stale reads**.

---

## 4. Leader-Leader Replication (Multi-Leader Replication)

### Architecture
- Multiple nodes can accept **write operations**.
- Each node replicates its changes to the others.

### Use Cases
- Systems where writes occur in **multiple locations** (e.g., collaborative tools, global databases).

### Advantages
- Higher **write availability** and **resilience**.
- Supports **geo-distributed writes**.

### Challenges
- **Conflict resolution** is complex.
  - Conflicts arise when two leaders accept conflicting writes before syncing.
- Common conflict strategies:
  - **Last-write-wins (LWW)**
  - **Custom merge functions**

---

## 5. Sharding (Data Partitioning)

### Definition
**Sharding** is the process of **splitting a large dataset into smaller, more manageable parts** called **shards**, which are stored across different machines.

Each shard contains a **subset of the data**, and together, all shards represent the **complete dataset**.

### Main Motivation
- **Scalability**: Handle more data than a single machine can store or process.
- **Performance**: Reduces the amount of data any single machine needs to handle.
- **Parallelism**: Queries can be processed in parallel across shards.

---

## 6. Basic Sharding Methods

### 6.1 Horizontal Sharding
- Rows of a table are divided among different shards.
- Each shard contains the same schema but a different subset of rows.

#### Example:
- User table split by `user_id` range:
  - Shard 1: user_id 1–1000
  - Shard 2: user_id 1001–2000

### 6.2 Vertical Sharding
- Different **columns** are stored in different shards.
- Each shard contains fewer columns of the same table.

#### Example:
- Shard 1: Stores personal info columns (`name`, `email`)
- Shard 2: Stores financial info columns (`credit_card`, `balance`)

### 6.3 Hash-Based Sharding
- Apply a **hash function** to a key (e.g., `user_id`) to determine the shard.
- Distributes data evenly and prevents skew.

#### Example:
```python
shard_index = hash(user_id) % num_shards
```

### 6.4 Range-Based Sharding
- Divide data based on key **ranges**.
- Simple but may lead to **data skew** if data is not uniformly distributed.

### 6.5 Directory-Based Sharding
- A **lookup service** (like a shard map) keeps track of where each piece of data resides.

#### Tradeoffs:
- Flexible but introduces **dependency on central service**.

---

