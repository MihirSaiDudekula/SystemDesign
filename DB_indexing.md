# Database Indexing: How It Speeds Up Reads

## Understanding Databases and Serialization
A **database** is a collection of **records** stored on disk. In **SQL databases**, records are rows grouped into tables, while in **document-based databases** like MongoDB, they are JSON documents. These records must be **serialized**—converted into a format suitable for disk storage. For example, in a SQL database, rows are stored sequentially, with each column occupying a specific number of bytes.

**Example: Users Table**
- **Columns**: 
  - **ID**: Integer (4 bytes)
  - **Name**: String (60 bytes)
  - **Age**: Integer (4 bytes)
  - **Bio**: String (128 bytes)
  - **Total Blocks**: Integer (4 bytes)
- **Row Size**: 4 + 60 + 4 + 128 + 4 = **200 bytes**
- For a table with **100 rows**, the total size is 200 × 100 = **20,000 bytes**.

## Disk Storage and Block Reads
Disks (magnetic or SSD) are divided into **blocks**, the smallest unit of data read during a **disk I/O** operation. Even if only a single byte is needed, the entire block containing that byte is read into memory.

**Example: Block Size**
- Assume a block size of **600 bytes**.
- Each row is 200 bytes, so one block can store **3 rows** (600 ÷ 200 = 3).
- For 100 rows, the number of blocks required is 100 ÷ 3 = **34 blocks** (rounded up from 33.3, as blocks are indivisible).

**Disk I/O Process**:
- When reading data, the disk identifies the block containing the requested bytes, loads it into memory, extracts the required data, and discards the rest.
- **Key Concept**: **Block reads** are the primary factor in read performance, as each block read incurs a time cost (e.g., hypothetically 1 second per block).

**Full Table Scan**:
- To read the entire table (100 rows, 34 blocks) row by row, the database performs **34 disk I/Os**, taking **34 seconds** (assuming 1 second per block).

**Query Example**: Find all users with **age = 23**.
- Without an index, the database performs a **full table scan**, reading all 34 blocks to check each row’s age, taking **34 seconds**.

## What Are Indexes?
An **index** is a data structure that improves query performance by mapping **indexed values** to **row references** (e.g., row IDs). It is a smaller, sorted table stored on disk, reducing the number of blocks read during queries.

**Index Structure**:
- A simple index on the **age** column is a two-column table:
  - **First Column**: Age (e.g., 4 bytes for an integer).
  - **Second Column**: Row ID (e.g., 4 bytes for an integer).
- **Index Entry Size**: 4 + 4 = **8 bytes**.
- For 100 rows, the index has **100 entries**, totaling 8 × 100 = **800 bytes**.
- With a 600-byte block size, the index requires **2 blocks** (600 bytes for the first 75 entries, 200 bytes for the remaining 25).

**Key Property**: The index is **sorted** by the indexed column (age), enabling efficient searches.

## How Indexes Speed Up Queries
Using an index reduces the number of **disk I/Os** by narrowing down the rows to read. For the query “find all users with age = 23”:

1. **Phase 1: Scan the Index**:
   - Read the entire index (2 blocks) to find entries where **age = 23**.
   - Collect the corresponding **row IDs** (e.g., ID 1 and ID 4).
   - **Cost**: **2 disk I/Os** (2 seconds, assuming 1 second per block).

2. **Phase 2: Fetch Relevant Rows**:
   - For each row ID, read the block containing the row from the main table.
   - Example: ID 1 is in the first block (rows 1–3), and ID 4 is in the second block (rows 4–6).
   - **Cost**: **2 disk I/Os** (one for each block containing the target rows).
   - Extract the full records and return them to the user.

**Total Cost**:
- Index scan: 2 blocks.
- Row retrieval: 2 blocks.
- Total: **4 disk I/Os** (4 seconds).

**Performance Gain**:
- Without an index: 34 seconds (full table scan).
- With an index: 4 seconds.
- **Speedup**: 34 ÷ 4 = **8.5x faster**.

## Optimizations and Real-World Considerations
- **Sorted Index**: Since the index is sorted by age, a query for **age = 23** can stop scanning once it passes age 23, potentially reducing the index scan to **1 block** (e.g., if age 23 entries are in the first block).
- **B+ Trees**: Real databases use **B+ trees** for indexes, enabling **multi-level indexing**. This allows **binary search**-like operations to locate relevant blocks faster, further reducing disk I/Os.
- **Storage Engine**: The database’s **storage engine** optimizes index storage and access, tailoring performance to the database type (e.g., SQL vs. NoSQL).

**Key Takeaway**: Indexes minimize **disk I/Os** by reducing the number of blocks read, significantly speeding up queries. Always ensure frequently queried columns are indexed to avoid **full table scans**, which can cripple performance in large databases.
