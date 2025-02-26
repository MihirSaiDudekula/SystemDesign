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

1. Key REST Concepts
A. Resources – Everything is a Resource
In REST, resources are the main objects we interact with.
Each resource is represented by a URL (Uniform Resource Locator).

For our e-commerce app, some resources are:

Users → /users
Products → /products
Orders → /orders
Cart → /cart
Each resource is managed using CRUD (Create, Read, Update, Delete) operations.

B. Stateless – Each Request is Independent
In REST, the server does not store any information about previous requests.
Each request must contain all necessary data (e.g., authentication token, user details).
Example (Stateless API Request)
A user wants to fetch their orders:

h
Copy
Edit
GET /orders HTTP/1.1
Host: example.com
Authorization: Bearer abc123
The server does not remember who the user is between requests.
The client must send the token (abc123) every time.
✅ Benefits:

Scalability – Any server can handle the request (no dependency on past requests).
No session storage on the server

Here's how you can implement the statelessness concept using query parameters:

1. Authentication with Query Parameters
In a stateless API, each request must independently provide enough information for the server to authenticate and authorize the request. This is typically done using an authentication token in the query parameters.

Example of a stateless request with query parameters for authentication:

http
Copy
GET /orders?auth_token=abc123 HTTP/1.1
Host: example.com
In this example, the auth_token query parameter carries the necessary authentication token (abc123) that the server uses to verify the identity of the user.
Every time the user sends a request, the auth_token must be included, as the server will not remember the token from any prior request.
2. Sending User-Specific Data in Query Parameters
When dealing with RESTful services, it's common to pass user-specific information like user_id or session_token via query parameters.

For example, if the user wants to fetch their orders, the request might look like this:

http
Copy
GET /orders?user_id=12345&auth_token=abc123 HTTP/1.1
Host: example.com
In this case:

user_id helps the server identify the user who is making the request.
auth_token is again necessary to authenticate the request, as the server doesn’t remember past requests or store session information.
3. Using Query Parameters for Pagination or Filters
In stateless REST APIs, you can include pagination, filtering, or sorting information in the query parameters to retrieve specific data. For instance, if a user is fetching a list of products but only wants to see products in a certain category and within a certain price range, you can use query parameters like this:

http
Copy
GET /products?category=electronics&min_price=100&max_price=1000&auth_token=abc123 HTTP/1.1
Host: example.com
category, min_price, and max_price are filters.
auth_token is required for authentication.
The server uses the query parameters to filter the results based on the specified values.

4. State in the Request (Session-Like Behavior without Storing State)
Although REST is stateless, you can mimic stateful behavior by sending the necessary "state" with every request using query parameters. For example, when a user adds an item to the shopping cart, the request might look like this:

http
Copy
GET /cart?user_id=12345&cart_token=xyz789&auth_token=abc123 HTTP/1.1
Host: example.com
user_id: Identifies the user.
cart_token: Identifies the user's shopping cart (acting like a session).
auth_token: Confirms the user's identity.
In this example, the cart_token helps the server identify the user's cart, and the auth_token ensures the request is coming from an authenticated user. Even though the server doesn’t store session data, the query parameters contain the "state" needed for the request.

5. Benefits of Statelessness with Query Parameters
Scalability: No session management on the server side, so any server can handle any request.
Security: Authentication and authorization information are always included in the request, reducing the risk of relying on potentially insecure session storage.
Flexibility: Each request can independently carry all the required information without depending on the server’s memory or previous requests.
Example Scenario
Let’s say a user wants to view their order history. Here’s how the statelessness would work:

The user logs in and receives an auth_token.
On every request, the user must include this auth_token to authenticate the request.
The user sends the following request to fetch their orders:
http
Copy
GET /orders?auth_token=abc123&user_id=12345 HTTP/1.1
Host: example.com
The server does not need to remember the user’s identity or any prior session information. It simply checks that the provided auth_token is valid and then returns the user's orders based on the user_id.

In summary, implementing statelessness using query parameters involves embedding all required context (such as authentication tokens, user data, and other stateful information) into each request via the query string.

Assume you want to fetch a list of videos. To make this stateless, you’ll pass a limit (number of videos per page) and an offset (starting point for the video list) as query parameters in the request URL. Additionally, you may need to pass an authentication token to ensure that the request is coming from an authenticated user.

Example Request: Fetching Videos
http
Copy
GET /videos?limit=10&offset=20&auth_token=abc123 HTTP/1.1
Host: example.com
limit=10: Fetches 10 videos at a time.
offset=20: Skips the first 20 videos and starts returning from the 21st video (used for pagination).
auth_token=abc123: Authentication token to verify the identity of the user.

---

### **B. GraphQL**
- **GraphQL is a query language** for APIs developed by Facebook.
- Uses **only POST requests** (unlike REST, which uses multiple methods).

- In GraphQL, all requests are made using the POST method, regardless of whether you're fetching data, modifying data, or performing other actions. This is in contrast to REST APIs, which use different HTTP methods for different operations
- body can contain:

A query (a read operation, like fetching specific fields of data).
A mutation (a write operation, like creating or updating data).
A subscription (to listen for real-time updates).
Because GraphQL operates with a single endpoint, POST is chosen to accommodate the flexibility and complexity of the request body. The request is sent to a server with the required query or mutation in the request body, and the server responds with the requested data.


What is Overfetching and Underfetching?
Overfetching and underfetching are common problems in traditional REST APIs. These issues arise from the way data is requested and delivered in REST, where you typically request a fixed set of data from an endpoint.

Overfetching
Overfetching occurs when an API returns more data than what the client actually needs. This can happen when an endpoint returns a larger set of data than the client requires, leading to inefficiencies like:

Unnecessary network usage (since more data is transferred than necessary).
Increased response size (more data than needed).
Slower performance for the client and server.
For example, in a REST API, a request to fetch a list of users might also return unnecessary information about each user, like their posts, comments, or other details that are irrelevant to the current use case.

Example of Overfetching in REST:
Request: GET /users
Response:
json
Copy
[
  { "id": 1, "name": "Alice", "email": "alice@example.com", "posts": [...] },
  { "id": 2, "name": "Bob", "email": "bob@example.com", "posts": [...] }
]
If the client only needs the user's name and email but the server still sends the posts, that's overfetching.

Underfetching
Underfetching occurs when an API doesn’t provide enough data in a single request, forcing the client to make multiple requests to get all the necessary information. This is a problem when an endpoint does not return all the relevant information in the response, requiring the client to fetch additional data by making multiple API calls.

For example, in a REST API, if you make a request to fetch user details but the server only returns basic information (like their name) and not their posts or other details, you will need to make a second request to get the posts or additional data.

Example of Underfetching in REST:
Request: GET /users/1
Response:
json
Copy
{
  "id": 1,
  "name": "Alice",
  "email": "alice@example.com"
}
To get posts, you'd need to make a second request: GET /users/1/posts.

How GraphQL Solves These Problems
In GraphQL, these issues are solved in the following ways:

No Overfetching: The client specifies exactly what data it needs in the query. The server will return only the data requested, so there's no unnecessary data included in the response.
No Underfetching: The client can request all the data it needs in a single query. If you need the user's name, email, and posts, you can request them all in one request, and the server will return exactly what’s needed


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

## **gRPC: High-Performance Remote Procedure Calls**  
gRPC is a **high-performance, open-source RPC (Remote Procedure Call) framework** developed by Google. It is commonly used for **server-to-server communication** rather than in browsers.

🔹 **Why gRPC?**  
- Works over **HTTP/2** (instead of HTTP/1.1 like REST).  
- Uses **Protocol Buffers (Protobuf)** instead of JSON (smaller, faster, binary format).  
- Supports **bi-directional streaming** (like WebSockets).  

---

## **1. Why gRPC Is Not Used Directly in Browsers?**
- Browsers **do not support gRPC natively** because it uses **HTTP/2 with binary encoding (Protobuf)**.
- Instead, browsers use **gRPC-Web**, which requires a **proxy** to translate gRPC calls to HTTP-friendly requests.

### **How Does gRPC Work with a Browser?**
1. **Browser sends a request** using `gRPC-Web`.
2. **A proxy (like Envoy) converts** it into a gRPC request.
3. **Server processes the request** and responds using gRPC.
4. **Proxy converts response** back to an HTTP response for the browser.

✅ **gRPC works great for microservices but requires a proxy for web apps.**

---

## **2. Protocol Buffers (Protobuf) – Why Not JSON?**
gRPC uses **Protocol Buffers (Protobuf)** instead of **JSON** for efficiency.

🔹 **JSON Drawbacks in REST APIs:**  
- **Human-readable but large** (extra characters like `{}`, `""` make it bulky).  
- **Slower to parse** because it’s text-based.  

🔹 **Protobuf Advantages in gRPC:**  
- **Binary format (smaller than JSON)** – Uses compact numbers instead of text.  
- **Schema-based** – Defines the structure upfront, making parsing efficient.  
- **Strongly typed** – Prevents data structure errors.

---

## **3. Defining a gRPC Service with Protobuf**
**Example: User Service using Protobuf**
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

### **How This Works:**
1. **Protobuf defines the API contract** (like an OpenAPI spec in REST).
2. The **server implements the functions** (e.g., `GetUser`).
3. The **client calls the functions as if they were local**.

✅ **This avoids manually defining REST endpoints and request formats.**

---

## **4. gRPC Supports Bi-Directional Streaming (Like WebSockets)**
Unlike REST (which is **request-response**), **gRPC supports streaming** over HTTP/2.

### **gRPC Streaming Types**
| Type | Description | Example |
|------|------------|---------|
| **Unary** | Client sends 1 request, server sends 1 response (like REST) | Get user info |
| **Server Streaming** | Server sends multiple responses to 1 request | Live stock prices |
| **Client Streaming** | Client sends multiple requests, server responds once | Uploading logs |
| **Bi-Directional Streaming** | Both client & server send multiple messages (like WebSockets) | Chat application |

### **Example: Bi-Directional Chat Streaming**
```proto
service ChatService {
  rpc ChatStream (stream ChatMessage) returns (stream ChatMessage);
}

message ChatMessage {
  string user = 1;
  string message = 2;
}
```
- **Client and server keep the connection open**.
- Messages are continuously exchanged **without new requests**.
- Similar to **WebSockets**, but **more efficient using HTTP/2**.

✅ **Use Case:** Real-time chat, collaborative editing, live stock updates.

---

## **5. Tradeoffs: gRPC vs REST vs WebSockets**
| Feature | gRPC | REST | WebSockets |
|---------|------|------|-----------|
| **Protocol** | HTTP/2 | HTTP/1.1 | TCP |
| **Data Format** | Protobuf (Binary) | JSON (Text) | Custom |
| **Performance** | Fast | Slower | Fast |
| **Streaming** | ✅ Yes | ❌ No | ✅ Yes |
| **Browser Support** | ❌ No (needs proxy) | ✅ Yes | ✅ Yes |
| **Human-Readable** | ❌ No | ✅ Yes | ❌ No |

✅ **gRPC is best for microservices & high-performance APIs.**  
❌ **Use REST if you need human-readable APIs & browser support.**  
❌ **Use WebSockets if you need full-duplex communication but not Protobuf.**

---

## **6. When Should You Use gRPC?**
✅ **Best for:**  
✔ Microservices (fast, low-latency communication).  
✔ Real-time applications (streaming).  
✔ Mobile & IoT (efficient, small payloads).  

❌ **Avoid if:**  
- Your API is **mainly used by browsers** (use REST or gRPC-Web with a proxy).  
- You need **easy debugging** (JSON is easier to read than binary Protobuf).  

---

## **Conclusion: gRPC Is Powerful but Not for Everything**
- **Faster, smaller, and supports streaming** (great for microservices).
- **Requires Protobuf & HTTP/2**, making debugging harder than REST.
- **Browsers don’t support it directly**, so a proxy is needed for frontend apps.


## **Summary**
| API Paradigm | Stateless/Stateful | Issues | Best For |
|-------------|------------------|--------|----------|
| **REST** | Stateless | Overfetching, Underfetching | General APIs |
| **GraphQL** | Stateless | Requires a GraphQL server | Flexible APIs, Avoiding Overfetching |
| **gRPC** | Stateful (Streaming Support) | Requires Protobuf, Proxy for Browsers | High-performance APIs, Microservices |


