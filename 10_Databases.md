
---

# Relational Database Systems and Associated Concepts

---

## 1. Relational Database Management System (RDBMS)

### Definition
A **Relational Database Management System (RDBMS)** is a type of database management system that stores data in the form of **relations** (i.e., tables). Each **table** consists of **rows (records/tuples)** and **columns (attributes)**.

### Key Characteristics
- Data is **structured** and adheres to a **predefined schema**.
- Relationships between data entities are maintained through **foreign keys**.
- Designed to support **ACID properties** to ensure data integrity.

### Common RDBMS Examples
- MySQL
- PostgreSQL
- Oracle DB
- Microsoft SQL Server

---

## 2. Structured Query Language (SQL)

SQL is the standard language used to interact with relational databases.

### SQL Categories

1. **DDL (Data Definition Language)** – for defining schema
   - `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`

2. **DML (Data Manipulation Language)** – for data operations
   - `INSERT INTO`, `UPDATE`, `DELETE`

3. **DQL (Data Query Language)** – for querying data
   - `SELECT`

4. **DCL (Data Control Language)** – for permission control
   - `GRANT`, `REVOKE`

### SQL Example
```sql
SELECT name, age FROM Employees WHERE department = 'Engineering';
```

This query retrieves the `name` and `age` of employees from the `Employees` table who belong to the Engineering department.

---

## 3. Disk Storage and Indexing

### Disk Storage in Databases
- Data in RDBMS is stored in **disk blocks** or **pages**.
- Disk access is **significantly slower** than memory access.
- To optimize performance, databases use **buffer managers** to cache frequently accessed pages.

### B+ Tree Indexing

#### Definition
A **B+ Tree** is a self-balancing tree data structure used for indexing in databases. It maintains sorted data and allows searches, sequential access, insertions, and deletions in **logarithmic time**.

#### Structure
- Internal nodes contain **keys** used for navigation.
- **All data records are stored at the leaf level**.
- Leaf nodes are **linked** for efficient **range queries**.

#### Advantages
- Efficient use of disk blocks (each node fits within a disk page).
- Supports **ordered traversal**.
- **Fewer levels** due to high fan-out (number of children per node), reducing I/O operations.

#### Example Use Case
Index on the `employee_id` column allows quick retrieval of employee records without full table scan.

---

## 4. ACID Properties

ACID is a set of properties that ensure reliable processing of database transactions.

1. **Atomicity**
   - A transaction is an indivisible unit: it either completes entirely or not at all.
   - Example: In a bank transfer, money must be debited from one account and credited to another; both operations must succeed or fail together.

2. **Consistency**
   - Ensures the database moves from one valid state to another, preserving **integrity constraints**.
   - Example: If a constraint says age must be ≥ 0, an update to -5 should be rejected.

3. **Isolation**
   - Transactions must appear to run independently, even when executed concurrently.
   - **Concurrency control techniques** like **locking**, **MVCC (Multi-Version Concurrency Control)**, or **serializability protocols** are used.

4. **Durability**
   - Once a transaction commits, its changes are permanent, even in the event of a crash.
   - Achieved via **write-ahead logging (WAL)** and **replication**.

---

# Non-Relational Databases (NoSQL)

---

## 5. What is NoSQL?

**NoSQL (Not Only SQL)** databases are non-relational systems designed to handle **large-scale, distributed, and schema-less** data.

### Motivation
- Traditional RDBMSs do not scale easily across multiple servers.
- NoSQL databases are designed to be **highly available**, **scalable**, and **flexible** in data modeling.

---

## 6. Types of NoSQL Databases

### 6.1 Key-Value Stores
- Data is stored as a collection of **key-value pairs**.
- Optimized for **fast lookups** by key.

#### Example
- `user_id: { "name": "Alice", "email": "alice@example.com" }`
- Common Use Cases: Caching, session management.
- Tools: **Redis**, **Amazon DynamoDB**

### 6.2 Document Stores
- Store data in **documents** (typically JSON, BSON).
- Each document is a self-contained data structure.
- Flexible schema – different documents in a collection may have different fields.

#### Example
```json
{
  "name": "Alice",
  "age": 28,
  "address": {
    "city": "Mumbai",
    "zip": "400001"
  }
}
```
- Tools: **MongoDB**, **Couchbase**

### 6.3 Wide-Column Stores
- Store data in **rows and columns**, but unlike RDBMS, columns are grouped into **column families**.
- Highly efficient for **write-heavy workloads** and **time-series data**.

#### Example
Row Key: `user123`  
Column Family: `Personal_Info`  
- name: Alice  
- email: alice@example.com

- Tools: **Apache Cassandra**, **HBase**

### 6.4 Graph Databases
- Data is represented as **nodes** (entities) and **edges** (relationships).
- Optimized for relationship-heavy queries.

#### Example
- Nodes: Users
- Edges: Friendships
- Query: Find mutual friends between two users.

- Tools: **Neo4j**, **Amazon Neptune**

---

## 7. Tradeoffs Between RDBMS and NoSQL

| Feature              | RDBMS                          | NoSQL                                 |
|----------------------|--------------------------------|----------------------------------------|
| **Schema**           | Strict, predefined             | Flexible or schema-less                |
| **Consistency Model**| Strong (via ACID)              | Eventual (via BASE)                    |
| **Scalability**      | Vertical (scale-up)            | Horizontal (scale-out)                 |
| **Transactions**     | Multi-step, reliable           | Often limited to single-partition ops  |
| **Use Cases**        | Banking, ERP, CRM              | Real-time analytics, IoT, big data     |
| **Joins**            | Supported                      | Generally not supported                |

---

## 8. ACID in NoSQL Databases

### Do NoSQL Systems Use ACID?

Most NoSQL databases **do not implement full ACID** guarantees, particularly in **distributed environments**. Instead, they follow **BASE** properties for performance and scalability.

---

## 9. BASE Properties

**BASE** stands for:

1. **Basically Available**
   - System guarantees availability, even in case of partial failures.

2. **Soft State**
   - System state may change over time without external input due to **eventual propagation**.

3. **Eventually Consistent**
   - All updates will propagate and the system will eventually reach a **consistent state**.
   - Acceptable in use cases where **immediate consistency is not critical** (e.g., social media likes, analytics).

---

## 10. Leader-Follower Model in Distributed Databases

### Definition
A commonly used **replication strategy** in distributed databases to improve **read scalability** and **fault tolerance**.

### Structure
- **Leader (Primary Node)**:
  - Accepts **all writes and updates**.
- **Followers (Replica Nodes)**:
  - Receive updates from the leader and serve **read requests**.

### Tradeoffs
- **Improves read scalability**, but
- **Leads to stale reads** unless strong consistency is enforced.

---
