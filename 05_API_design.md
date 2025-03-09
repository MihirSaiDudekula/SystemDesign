### **API Design: A System Design Perspective**  

Designing an API from scratch requires careful planning to ensure scalability, maintainability, and usability. Here’s a structured approach with a detailed explanation of key API design principles.

---

## **1. Identifying Entities and Verbs (CRUD)**
Before designing an API, we must identify:  

- **Entities**: The core resources our system will manage (e.g., users, products, orders).  
- **Verbs (Actions)**: The operations clients can perform on these entities, often mapped to CRUD operations:  

| HTTP Method | Operation | Description |
|-------------|----------|-------------|
| **POST**    | Create   | Add a new entity |
| **GET**     | Read     | Retrieve entity data |
| **PUT**     | Update   | Modify an entity (full update) |
| **PATCH**   | Update   | Modify an entity (partial update) |
| **DELETE**  | Delete   | Remove an entity |

### **Example: Designing an API for a Blog System**
Entities:  
1. `User` (id, name, email)  
2. `Post` (id, title, content, author_id)  
3. `Comment` (id, text, post_id, user_id)

API Endpoints:

- `POST /users` → Create a new user  
- `GET /users/{id}` → Retrieve user details  
- `POST /posts` → Create a new post  
- `GET /posts` → Retrieve all posts  
- `GET /posts/{id}` → Retrieve a single post  
- `PUT /posts/{id}` → Update a post  
- `DELETE /posts/{id}` → Delete a post  
- `POST /posts/{id}/comments` → Add a comment to a post  
- `GET /posts/{id}/comments` → Retrieve comments for a post  

---

## **2. API Changes: Can We Modify Public APIs?**
### **Why is modifying public APIs problematic?**  
Once an API is released to the public, clients (mobile apps, web apps, third-party services) integrate it into their applications. If we modify or remove existing functionality, it can break their applications, leading to bad user experiences or loss of service.

### **Challenges in modifying public APIs**  
- **Breaking Changes**: Removing or renaming fields, changing data formats, or altering authentication mechanisms can break client applications.  
- **Backward Compatibility Issues**: If older clients expect certain fields or behaviors, updates might cause failures.  
- **Dependency on Older Versions**: Some clients might be built with older API versions and might not upgrade immediately.  

### **Solutions to Manage API Changes**
1. **Versioning**  
   - **URL Versioning**  
     - Example: `https://api.example.com/v1/users` → Later, introduce `v2`: `https://api.example.com/v2/users`
     - Pros: Explicit and easy to implement  
     - Cons: Duplicates code and can cause maintenance issues  
   - **Header Versioning**  
     - Example: `Accept: application/vnd.example.v2+json`  
     - Pros: Keeps URLs clean and reduces code duplication  
     - Cons: Harder for clients to detect changes  
   - **Query Parameter Versioning**  
     - Example: `https://api.example.com/users?version=2`  
     - Pros: Simple to implement  
     - Cons: Less common and can clutter URLs  

2. **Deprecation Strategy**  
   - Mark fields or endpoints as "deprecated" before removing them  
   - Provide clear documentation and timelines for removal  
   - Example:  
     ```json
     {
       "name": "John Doe",
       "email": "john@example.com",
       "age": "Deprecated: Use 'birthdate' instead"
     }
     ```

3. **Feature Flags**  
   - Allow new features to be toggled on/off dynamically.  
   - Clients can opt into new features without breaking old ones.  

---

## **3. API Endpoints and Bodies**
An API **endpoint** is a specific URL where the client interacts with the server, like:
- `https://api.example.com/users`
- `https://api.example.com/posts/12/comments`

### **What’s Passed in the URL vs. the Body?**
| Data Type      | URL (Path/Query Parameter) | Body |
|---------------|-------------------------|------|
| **Resource Identifier** | `/users/{id}` (`id` in URL) | - |
| **Filtering Criteria** | `/posts?author_id=3` (Query Param) | - |
| **Pagination** | `/posts?limit=10&offset=20` | - |
| **New Resource Data** | - | `{ "name": "John Doe", "email": "john@example.com" }` |
| **Updating Data** | - | `{ "title": "New Title" }` |

**Rule of Thumb**:  
- **Use URL parameters** for resource identification.  
- **Use query parameters** for filtering, sorting, and pagination.  
- **Use the body** for sending new/updated data (except for GET requests).

---

## **4. Pagination and Associated Parameters**
Large datasets require pagination to improve performance. Common pagination techniques:

### **Offset-Based Pagination**
- **Example**: `GET /posts?limit=10&offset=20`
- **Parameters**:  
  - `limit` (number of items per page)  
  - `offset` (number of items to skip)  
- **Cons**: Expensive for large datasets.

### **Cursor-Based Pagination**
- **Example**: `GET /posts?cursor=abc123&limit=10`
- Uses a unique identifier (`cursor`) to fetch the next set of results.
- **Pros**: More efficient for large datasets.

---

## **5. Idempotence of GET Requests and Caching**
### **What is Idempotence?**  
An API request is **idempotent** if making multiple identical requests has the same effect as making a single request.

| HTTP Method | Idempotent? | Reason |
|-------------|------------|--------|
| **GET**    | ✅ Yes | Does not change state |
| **POST**   | ❌ No | Creates a new resource each time |
| **PUT**    | ✅ Yes | Updates resource to the same state |
| **DELETE** | ✅ Yes (mostly) | Deleting an already deleted item has no further effect |

### **Caching GET Requests**
- Caching helps reduce API load and response times.
- HTTP Caching Headers:
  - `Cache-Control: max-age=3600` → Cache for 1 hour
  - `ETag: "abc123"` → Response versioning
  - `If-Modified-Since: <timestamp>` → Returns `304 Not Modified` if unchanged

---

## **6. Client-Side vs. Server-Side Processing**
### **Client-Side Processing**  
The client (browser, mobile app, frontend app) handles:  
- Rendering UI  
- Making API requests  
- Validating input before sending it to the server  
- Caching data locally (e.g., storing tokens, session data)  

### **Server-Side Processing**  
The server handles:  
- Business logic and security  
- Database operations (CRUD)  
- Authentication and authorization  
- Rate limiting and logging  

### **Key Differences**  
| Task | Client-Side | Server-Side |
|------|------------|-------------|
| **Form Validation** | ✅ (JavaScript validation) | ✅ (Backend validation required) |
| **Data Processing** | ✅ (UI rendering, minor computation) | ✅ (Business logic, DB operations) |
| **Authentication** | ❌ (Tokens stored but not processed) | ✅ (JWT, OAuth verification) |
| **API Calls** | ✅ (AJAX, Fetch) | ✅ (Processes requests, returns responses) |

---

### **Final Thoughts**
API design is an iterative process that balances usability, scalability, and maintainability. Keeping APIs **RESTful, versioned, paginated, and idempotent** ensures a stable and scalable system.
