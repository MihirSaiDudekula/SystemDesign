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

### **Python Implementation of Consistent Hashing**  
```python
import hashlib
import bisect

class ConsistentHashing:
    def __init__(self, num_replicas=3):
        self.num_replicas = num_replicas
        self.ring = {}  # Maps hash values to servers
        self.sorted_keys = []  # Sorted list of hash values

    def _hash(self, key):
        """Returns a consistent hash for a given key."""
        return int(hashlib.md5(key.encode()).hexdigest(), 16) % (2**32)

    def add_server(self, server):
        """Adds a server to the hash ring."""
        for i in range(self.num_replicas):
            hash_value = self._hash(f"{server}-{i}")
            self.ring[hash_value] = server
            bisect.insort(self.sorted_keys, hash_value)

    def remove_server(self, server):
        """Removes a server from the hash ring."""
        for i in range(self.num_replicas):
            hash_value = self._hash(f"{server}-{i}")
            if hash_value in self.ring:
                self.ring.pop(hash_value)
                self.sorted_keys.remove(hash_value)

    def get_server(self, request_key):
        """Finds the server for the given request using consistent hashing."""
        if not self.ring:
            return None
        
        hash_value = self._hash(request_key)
        idx = bisect.bisect(self.sorted_keys, hash_value) % len(self.sorted_keys)
        return self.ring[self.sorted_keys[idx]]

# Example Usage
ch = ConsistentHashing()
ch.add_server("ServerA")
ch.add_server("ServerB")
ch.add_server("ServerC")

print(ch.get_server("192.168.1.10"))  # Should return the same server for the same IP
```

---

### **Benefits of Consistent Hashing in Load Balancing**  
✅ **Minimizes key movement** when servers are added/removed.  
✅ **Ensures session persistence** for stateful requests.  
✅ **Improves cache efficiency** by reducing cache misses.  

---
