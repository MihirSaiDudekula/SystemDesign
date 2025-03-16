# **Consistent Hashing in Load Balancers**  

## **Why Use Consistent Hashing?**  
In traditional **Round Robin** or **Least Connections** load balancing, a request can be routed to **any server**, potentially leading to:  
- **State Loss**: If session data is stored locally on a server, switching servers may cause session loss.  
- **Cache Inefficiency**: If cached data exists on one server but the next request goes to another server, it results in cache misses.  

### **How Consistent Hashing Helps**  
- Ensures that **requests from the same IP** (or with the same identifier) go to the **same server** most of the time.  
- Evenly distributes requests **without overloading specific servers**.  
- When servers are added/removed, only **a small fraction of keys** need redistribution.  

---

## **The Circle Method (Hash Ring) Implementation**  

### **Concept**  
- Each server and request is assigned a **hash value** (via a hash function).  
- These values are placed on a **circular space** (0 to 2³²-1 for a 32-bit hash).  
- A request is mapped to the nearest server **clockwise** in the ring.  

### **Steps to Implement**  
1. **Assign a hash value to each server** using a **consistent hash function** (e.g., `hash(server_IP)`).  
2. **Assign a hash value to incoming requests** using the same function (e.g., `hash(client_IP)`).  
3. Place both **servers and requests** on a **hash ring**.  
4. Route each request to the **next available server clockwise**.  
5. If a server is removed, redistribute requests to the **next available server**.  

---

# **Understanding Consistent Hashing with a Simple Example**

### **Problem with Traditional Load Balancing**  
Consider a **traditional load balancer** that distributes incoming requests to servers **randomly** or using **round-robin**.  
If **Server A** is removed or added, many requests will get **redirected to a different server**, causing:  
- **Session Loss** (if session data is stored on individual servers).  
- **Cache Inefficiency** (requests may no longer go to the server where cached data is stored).  

### **How Consistent Hashing Helps**  
Instead of **randomly assigning** requests to servers, we:  
1. **Assign a unique hash** to each server and request.  
2. Place them on a **circular ring** (hash space).  
3. Each request is assigned to the **next server clockwise**.  

If a **server is removed**, only requests assigned to that server need **redistribution**—other requests remain mapped **as before**.  

---

## **Step-by-Step Explanation of Consistent Hashing**  

### **1. Creating a Hash Ring**  
Imagine a **circular space** (hash ring) with values ranging from **0 to 100** (for simplicity).  

We add **three servers**:  
- `Server A` is hashed to position **20**  
- `Server B` is hashed to position **60**  
- `Server C` is hashed to position **90**  

**Diagram Representation:**  

```
  0         20       60       90       100
  |---------|--------|--------|--------|
           A        B        C
```

---

### **2. Assigning Requests to Servers**  
Now, let's say a **client request is hashed** to a value of **50**.  
- We move **clockwise** to find the next available server.  
- `50` is between `20 (A)` and `60 (B)`, so it goes to `Server B`.  

```
  0         20       50       60       90       100
  |---------|--------x--------|--------|--------|
           A                 B        C
```

Another request hashes to **85**:  
- `85` is between `60 (B)` and `90 (C)`, so it goes to `Server C`.  

---

### **3. What Happens When a Server is Removed?**  
If **Server B is removed**, all requests mapped to `B` will go to the **next server clockwise (`C`)**.  
- Requests mapped to **Server A or C remain unchanged**.  

```
  0         20       50       60       90       100
  |---------|--------x--------|--------|--------|
           A                 (B removed) C
```

Now, **50 (previously going to B) will go to C instead**.  

---

### **4. What Happens When a New Server is Added?**  
Now, suppose we add **Server D**, which hashes to **40**.  
- Requests previously going to **B** will now be split between **B and D**.  

```
  0         20       40       50       60       90       100
  |---------|--------|--------x--------|--------|--------|
           A        D                 B        C
```

✅ **Minimal data movement!** Only requests **between A and B** are affected.  

---

## **Advantages of Consistent Hashing**  
🔹 **Minimizes request reassignment** when servers are added/removed.  
🔹 **Efficient load distribution** with **fewer disruptions**.  
🔹 **Works well for cache servers (e.g., Redis, Memcached)**.  

### **Use Cases**  
✔ **Load balancing (Nginx, AWS ELB)**  
✔ **Distributed databases (Cassandra, DynamoDB)**  
✔ **Content Delivery Networks (CDNs)**  
