## **1. APIs (Application Programming Interfaces)**
- APIs define how different software components communicate.
- Used for data exchange between clients (frontend, mobile apps) and servers.
- API paradigms include **REST, GraphQL, and gRPC**.

---

## **2. API Paradigms**

### **A. REST (Representational State Transfer)**
- **REST is an architectural style** for designing web APIs.
- Based on **resources**, each identified by a URL.
- Uses **HTTP methods** (`GET`, `POST`, `PUT`, `DELETE`) to perform actions on resources.

### **Key REST Concepts:**
1. **Resources** – Everything is a resource (e.g., `/users`, `/orders`).
2. **Stateless** – Each request is independent; the server doesn’t store client state.
3. **Client-Server Separation** – The client only requests data, the server provides it.
4. **Cacheable** – Responses can be cached for efficiency.

### **RESTful API Example:**
| HTTP Method | Endpoint | Action |
|------------|---------|---------|
| `GET` | `/users` | Fetch all users |
| `POST` | `/users` | Create a new user |
| `GET` | `/users/{id}` | Get a specific user |
| `PUT` | `/users/{id}` | Update user info |
| `DELETE` | `/users/{id}` | Delete a user |

### **Stateful vs Stateless APIs**
- **Stateless APIs** – Each request is independent (REST APIs follow this).
- **Stateful APIs** – The server maintains session state between requests (e.g., WebSockets, some RPCs).

### **REST API Issues:**
- **Overfetching** – The client gets more data than needed.
- **Underfetching** – The client needs multiple requests to get required data.

---

### **B. GraphQL**
- **GraphQL is a query language** for APIs developed by Facebook.
- Uses **only POST requests** (unlike REST, which uses multiple methods).
- **Solves REST’s overfetching and underfetching problems**.

### **How GraphQL Works:**
1. **Client sends a single request** with a query specifying required fields.
2. **Server processes the query** and returns **only the requested data**.

#### **GraphQL Query Example (Client Request)**
```graphql
query {
  user(id: 1) {
    name
    email
    posts {
      title
    }
  }
}
```

#### **GraphQL Response (Server)**
```json
{
  "data": {
    "user": {
      "name": "Alice",
      "email": "alice@example.com",
      "posts": [
        { "title": "GraphQL Basics" }
      ]
    }
  }
}
```
✅ **No overfetching or underfetching** – The client specifies exactly what it needs.

### **GraphQL vs REST**
| Feature | REST | GraphQL |
|---------|------|---------|
| HTTP Methods | GET, POST, PUT, DELETE | Only POST |
| Overfetching | ✅ Yes | ❌ No |
| Underfetching | ✅ Yes | ❌ No |
| Response Format | Fixed JSON structure | Custom structure |
| Performance | Multiple requests | Single optimized request |

---

### **C. gRPC (Google Remote Procedure Call)**
- **gRPC** is an **RPC framework** by Google for high-performance communication.
- Uses **Protocol Buffers (Protobuf)** instead of JSON (more efficient).
- Works over **HTTP/2** (supports multiplexing multiple requests).

### **How gRPC Works:**
1. **Client calls a remote function**.
2. **Request is serialized using Protobuf** (efficient binary format).
3. **Server processes the request** and returns a serialized response.
4. **Client deserializes the response**.

### **Example gRPC Definition (Protobuf)**
```proto
syntax = "proto3";

service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
}

message UserRequest {
  int32 id = 1;
}

message UserResponse {
  string name = 1;
  string email = 2;
}
```

### **gRPC vs REST**
| Feature | REST | gRPC |
|---------|------|------|
| Protocol | HTTP 1.1 | HTTP/2 |
| Data Format | JSON (Text) | Protobuf (Binary) |
| Performance | Slower | Faster |
| Streaming | ❌ No | ✅ Yes |
| Human-Readable | ✅ Yes | ❌ No (Binary) |

### **Why gRPC Requires a Proxy**
- **Browsers do not natively support gRPC** (because it uses HTTP/2 and binary encoding).
- Requires a **gRPC-Web proxy** to communicate with frontend clients.

---

## **Summary**
| API Paradigm | Stateless/Stateful | Issues | Best For |
|-------------|------------------|--------|----------|
| **REST** | Stateless | Overfetching, Underfetching | General APIs |
| **GraphQL** | Stateless | Requires a GraphQL server | Flexible APIs, Avoiding Overfetching |
| **gRPC** | Stateful (Streaming Support) | Requires Protobuf, Proxy for Browsers | High-performance APIs, Microservices |

Would you like **a deeper comparison of GraphQL vs REST vs gRPC with real-world use cases?** 🚀
