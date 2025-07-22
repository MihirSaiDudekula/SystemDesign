# Advanced Notes on B+ Trees in Databases

## Introduction to Database Storage
Databases, both SQL and NoSQL (e.g., MongoDB with its **WiredTiger** storage engine), commonly use **B+ trees** to store data efficiently on disk. This data structure optimizes operations like insert, update, delete, and range queries compared to naive storage methods. The following sections explore why B+ trees are preferred, how data is stored in them, and their serialization on disk.

## Naive Storage Approach
In a simplistic storage model, data is stored sequentially in a single file, with rows (or documents) placed one after another. This approach has significant limitations for database operations:

- **Insert**: To maintain order by **primary key**, inserting a new row in the middle of a file requires creating a new file, copying all preceding rows, adding the new row, and then copying the remaining rows. This results in an **O(n)** time complexity due to the need to rewrite the entire file.
- **Update**: Updating a row requires a linear scan to locate it (**O(n)**), and overwriting is restricted to the exact byte width of the row to avoid corrupting adjacent data. If the updated row size exceeds the original, a new file must be created, similar to insertion.
- **Find**: Locating a specific row by ID involves a linear scan (**O(n)**) since there are no indexes to optimize the search.
- **Delete**: Deleting a row requires finding it (**O(n)**), then creating a new file by copying all rows except the one to be deleted, as disk-based file I/O does not allow in-place deletion without overwriting.
- **Range Queries**: While range queries can leverage sequential access once the starting row is found, locating the first row still requires an **O(n)** linear scan.

This naive approach is inefficient for transactional databases, necessitating a more optimized data structure like the **B+ tree**.

## Why B+ Trees?
**B+ trees** address the inefficiencies of naive storage by organizing data in a way that minimizes disk I/O and supports efficient operations. They are balanced trees with the following key characteristics:

- **Leaf Nodes**: Store actual data (rows or documents) in sorted order by **primary key**.
- **Non-Leaf Nodes**: Contain **routing information** (range pointers) to guide searches to the appropriate leaf nodes.
- **Linearly Linked Leaf Nodes**: Allow efficient traversal for **range queries**.
- **Node Size Alignment**: Each node is typically sized to match the **disk block size** (e.g., 4 KB), ensuring that disk reads/writes are optimized.

### B+ Tree Structure
A **B+ tree** consists of a hierarchy of nodes:
- **Root Node**: The topmost node, containing pointers to child nodes based on key ranges.
- **Internal (Non-Leaf) Nodes**: Store **range information** (e.g., minimum and maximum keys of child nodes) to direct searches.
- **Leaf Nodes**: Contain the actual table data (rows or documents), sorted by **primary key**, and are linked linearly to facilitate sequential access.
- **Disk Block Alignment**: Each node is sized to match the **disk block size** (e.g., 4 KB). For example, if a row is 40 bytes, a 4 KB node can store approximately 100 rows.

### Serialization on Disk
Data in a **B+ tree** is serialized and stored on disk as follows:
- Each **B+ tree node** is stored as a **disk block** (e.g., 4 KB), with its location identified by a **disk offset** in the database file.
- **Leaf nodes** store the actual row data, while **non-leaf nodes** store pointers (disk offsets) and key ranges.
- The database maintains a mapping of **disk offsets** to locate nodes, which may not be stored sequentially but are linked via pointers.
- The **linear linkage** of leaf nodes allows sequential access for range queries without traversing the tree repeatedly.

## B+ Tree Operations
**B+ trees** significantly improve the efficiency of database operations compared to the naive approach:

### Find Operation
To find a row by **primary key** (e.g., ID = 3):
1. Read the **root node** from disk to determine the key range (e.g., 1–201).
2. Follow the pointer to the appropriate **internal node** (e.g., 1–101).
3. Read the **leaf node** containing the range (e.g., 1–100).
4. Scan the leaf node (up to 100 rows) to locate the row with ID = 3.

This process requires **O(log n)** disk reads (proportional to the tree height, typically 2–3 levels for large datasets) plus a small in-memory scan, compared to **O(n)** for a linear scan in the naive approach.

### Insert Operation
To insert a row (e.g., ID = 4):
1. Traverse the tree (similar to find) to locate the appropriate **leaf node** (e.g., 1–100).
2. Read the leaf node into memory, insert the row at the correct position (e.g., after ID = 3), and shift subsequent rows.
3. Write the updated leaf node back to disk.
4. If the node exceeds capacity, split it and update parent nodes, potentially rebalancing the tree.

This requires **O(log n)** disk reads/writes, with in-memory adjustments, and avoids rewriting the entire file.

### Update Operation
To update a row (e.g., ID = 202):
1. Traverse to the appropriate **leaf node** (e.g., 201–300).
2. Read the leaf node into memory, update the row, and write the node back to disk.
3. If the update changes the row size significantly, a node split or rebalance may be needed.

This also operates in **O(log n)** time, with minimal disk I/O.

### Delete Operation
To delete a row (e.g., ID = 401):
1. Traverse to the **leaf node** containing the row (e.g., 301–400).
2. Read the node into memory, remove the row, and write the updated node back to disk.
3. Rebalance the tree if necessary to maintain **B+ tree properties** (e.g., minimum node occupancy).

This is also **O(log n)**, avoiding the need to rewrite the entire file.

### Range Queries
To retrieve rows in a range (e.g., IDs 100–600):
1. Traverse to the **leaf node** containing the starting key (e.g., ID = 100 in node 1–100).
2. Read the leaf node and collect rows until the end of the node.
3. Follow the **linear linkage** to the next leaf node (e.g., 101–200), continuing until the end of the range (e.g., 301–400 for ID = 600).
4. Return all rows in the range.

The **linear linkage** of leaf nodes makes range queries highly efficient, requiring **O(log n)** to locate the first node and **O(k)** to read subsequent nodes, where **k** is the number of nodes in the range.

## Advantages of B+ Trees
- **Efficient Disk I/O**: By aligning node size with **disk block size**, **B+ trees** minimize disk reads/writes.
- **Logarithmic Complexity**: Operations like find, insert, update, and delete are **O(log n)**, compared to **O(n)** in the naive approach.
- **Range Query Optimization**: **Linearly linked leaf nodes** enable sequential access, making range queries efficient.
- **Scalability**: **B+ trees** maintain balanced height, ensuring consistent performance for large datasets.
- **Versatility**: Used in both SQL (e.g., MySQL’s **InnoDB**) and NoSQL (e.g., MongoDB’s **WiredTiger**) databases.

## Technical Details
- **Disk Block Size**: Typically 4 KB, determining the size of each **B+ tree node**.
- **Node Capacity**: For a 4 KB node and 40-byte rows, approximately 100 rows can be stored per leaf node.
- **Tree Height**: A **B+ tree** with a branching factor of **m** (number of pointers per node) has a height of **log_m(n)**, where **n** is the number of rows, ensuring efficient traversal.
- **Rebalancing**: After insertions or deletions, the tree may split or merge nodes to maintain balance, adhering to **B+ tree properties** (e.g., each node must be at least half full).
