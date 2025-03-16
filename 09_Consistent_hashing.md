Consistent Hashing in Load Balancers
Why Use Consistent Hashing?
In traditional Round Robin or Least Connections load balancing, a request can be routed to any server, potentially leading to:

State Loss: If session data is stored locally on a server, switching servers may cause session loss.
Cache Inefficiency: If cached data exists on one server but the next request goes to another server, it results in cache misses.
How Consistent Hashing Helps
Ensures that requests from the same IP (or with the same identifier) go to the same server most of the time.
Evenly distributes requests without overloading specific servers.
When servers are added/removed, only a small fraction of keys need redistribution.
The Circle Method (Hash Ring) Implementation
Concept
Each server and request is assigned a hash value (via a hash function).
These values are placed on a circular space (0 to 2³²-1 for a 32-bit hash).
A request is mapped to the nearest server clockwise in the ring.
Steps to Implement
Assign a hash value to each server using a consistent hash function (e.g., hash(server_IP)).
Assign a hash value to incoming requests using the same function (e.g., hash(client_IP)).
Place both servers and requests on a hash ring.
Route each request to the next available server clockwise.
If a server is removed, redistribute requests to the next available server.
