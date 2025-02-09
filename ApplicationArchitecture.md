---

## **1. Developer Workflow: Build & Deploy Code**
### **1.1. Development**
- A **developer** writes code for a web application using:
  - **Frontend**: HTML, CSS, JavaScript (React, Angular, Vue, etc.).
  - **Backend**: Node.js (Express), Python (Flask, Django), Java (Spring Boot), etc.
  - **Databases**: MySQL, PostgreSQL, MongoDB, etc.

### **1.2. Building & Deploying Code**
- **Build Process** (Compiling, Bundling, Minification)
  - Frontend frameworks bundle and optimize assets.
  - Backend code is compiled or packaged (if necessary).

- **Deployment**
  - Code is pushed to a **version control system** (e.g., GitHub, GitLab).
  - A **CI/CD pipeline** (Jenkins, GitHub Actions, GitLab CI) automates:
    - Testing
    - Packaging (Docker, JAR, etc.)
    - Deploying to a **server**.

---

## **2. Servers: Application Hosting**
### **2.1. Server as Another Computer**
A **server** is just another computer that runs the application. It can be:
- A **physical server** in a data center.
- A **virtual machine (VM)** running in the cloud (AWS EC2, Google Compute Engine).
- A **containerized application** running in **Docker** or **Kubernetes**.

### **2.2. Serving the Application**
Once deployed, the server:
- **Handles user requests** and returns responses.
- Can serve:
  - **Frontend assets** (HTML, CSS, JS).
  - **Backend API endpoints** (REST, GraphQL).

---

## **3. Storage: Database & File Storage**
### **3.1. Database (Relational or NoSQL)**
- The **server interacts with the database** over a **network**.
- Examples:
  - Relational (SQL): MySQL, PostgreSQL.
  - NoSQL: MongoDB, Redis.
  - Cloud Databases: Amazon RDS, Firebase.

### **3.2. File Storage**
- If the application needs to store images, videos, or logs:
  - Local Disk (not scalable).
  - Cloud Storage (Amazon S3, Google Cloud Storage).
  - Network File Storage (NFS, EFS in AWS).

---

## **4. User Accessing the Server**
Users interact with the server in **two ways**:

### **4.1. Frontend Request (Web Page)**
- The user opens a website (`https://example.com`).
- The browser requests:
  - `index.html`, `style.css`, `app.js` (Frontend files).
  - These files are served by the **server or a CDN (Content Delivery Network)**.

### **4.2. API Request**
- A user (or another service) calls an API endpoint (`https://example.com/api/users`).
- The server processes the request, interacts with the database, and returns JSON.

---

## **5. Scaling the Server**
When traffic increases, a single server may become a bottleneck. We have two options:

### **5.1. Vertical Scaling (Scaling Up)**
- Increase **CPU, RAM, storage** on a single machine.
- Simple but has limits (cost, hardware constraints).

### **5.2. Horizontal Scaling (Scaling Out)**
- Add **multiple servers** behind a **load balancer**.
- Each server handles part of the traffic.
- Can scale dynamically based on load.

---

## **6. Load Balancer: Distributing Traffic**
A **Load Balancer** ensures that no single server is overwhelmed. It:
- Distributes requests to the **server with the least traffic**.
- Can use **Round-Robin**, **Least Connections**, or other algorithms.
- Can handle **failover** (rerouting traffic if a server crashes).

Examples:
- **Cloud Load Balancers** (AWS ELB, Google Cloud LB).
- **Software Load Balancers** (NGINX, HAProxy).

---

## **7. Server-to-Server Communication**
A single server often needs to communicate with **other servers**:
- Calls external **API endpoints** (`https://thirdparty.com/api`).
- Uses **microservices** to interact with different services.
- Can use **message queues** (RabbitMQ, Kafka) for async processing.

Example:
- A **frontend request** to `https://example.com/profile` may:
  1. Call an **authentication service**.
  2. Fetch user details from a **user service**.
  3. Fetch user posts from a **posts service**.

---

## **8. Logging, Metrics, and Alerts**
### **8.1. Logging**
- Servers log important events (e.g., errors, requests).
- Stored in **files** or **logging services** (ELK Stack, Loki, CloudWatch).

### **8.2. Metrics**
- Track **CPU usage, memory, requests per second**.
- Tools: **Prometheus, Datadog, Grafana**.

### **8.3. Alerting**
- If a **server crashes** or **high CPU usage** occurs, an alert is triggered.
- Tools: **PagerDuty, Grafana Alerts, AWS CloudWatch Alarms**.

---
