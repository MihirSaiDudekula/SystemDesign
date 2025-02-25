When designing a system, understanding networking fundamentals is crucial for efficient communication between components. Let’s go over these key networking concepts systematically:

---

### **1. IP Addresses**
- **IP (Internet Protocol) Address**: A unique identifier assigned to a device on a network.
- **IPv4 vs. IPv6**:
  - **IPv4**: 32-bit address (e.g., `192.168.1.1`).
  - **IPv6**: 128-bit address (e.g., `2001:db8::ff00:42:8329`).
- **Public vs. Private IP**:
  - **Public IP**: Routable over the internet (e.g., assigned by an ISP).
  - **Private IP**: Used in local networks (`192.168.x.x`, `10.x.x.x`).
- **Static vs. Dynamic IP**:
  - **Static IP**: Manually assigned and does not change.
  - **Dynamic IP**: Assigned via **DHCP (Dynamic Host Configuration Protocol)** and changes periodically.

---

### **2. Internet Protocol Suite (TCP/IP Model)**
The **TCP/IP model** consists of 4 layers:
1. **Application Layer**: Protocols like HTTP, FTP, SMTP, DNS.
2. **Transport Layer**: TCP (reliable) and UDP (unreliable).
3. **Internet Layer**: IP (addressing & routing).
4. **Link Layer**: Physical transmission (Ethernet, Wi-Fi).

---

### **3. IP Protocol (Unreliable)**
- **IP (Internet Protocol)** is a **connectionless** protocol responsible for routing packets across networks.
- **Unreliable** means that:
  - No guarantee of packet delivery.
  - No error correction.
  - Packets may be lost, duplicated, or arrive out of order.
- IP relies on higher-layer protocols (e.g., TCP) for reliability.

---

### **4. TCP Protocol (Reliable)**
- **Transmission Control Protocol (TCP)** provides **reliable, ordered, and error-checked** communication.
- **3-Way Handshake** (establishes a connection):
  1. **SYN**: Client → Server (request to start communication).
  2. **SYN-ACK**: Server → Client (acknowledges request).
  3. **ACK**: Client → Server (final acknowledgment).
- **Sequence Number**:
  - Each TCP segment has a **sequence number** to track packet order.
  - Helps in reordering packets and detecting lost packets.

---

### **5. UDP Protocol (Unreliable)**
- **User Datagram Protocol (UDP)** is a **connectionless** and **faster** alternative to TCP.
- **No reliability mechanisms**:
  - No handshake, retransmission, or ordering.
  - Packets may be lost or received out of order.
- **Used for**:
  - Streaming (e.g., video, VoIP).
  - Gaming (real-time interactions).

---

### **6. Application Layer Protocol - HTTP and Its Methods**
- **Hypertext Transfer Protocol (HTTP)** enables communication between web clients and servers.
- **Common HTTP Methods**:
  - `GET`: Retrieve a resource.
  - `POST`: Submit data to be processed.
  - `PUT`: Update a resource.
  - `DELETE`: Remove a resource.
  - `HEAD`: Retrieve headers only.
  - `PATCH`: Partially update a resource.

- **HTTPS**: Secure version of HTTP, using TLS for encryption.

---

### **7. Static vs. Dynamic IPs with NAT**
- **Static IP**: Manually assigned, used for servers.
- **Dynamic IP**: Assigned by **DHCP** and changes over time.
- **Network Address Translation (NAT)**:
  - Translates private IPs to a public IP for internet access.
  - **Types**:
    - **SNAT (Source NAT)**: Maps multiple private IPs to a single public IP.
    - **DNAT (Destination NAT)**: Maps an external request to an internal server.

---

### **8. Domain Name System (DNS)**
- **DNS** resolves human-readable domain names (e.g., `google.com`) to IP addresses.
- **DNS Query Process**:
  1. User requests `google.com`.
  2. Browser checks local cache.
  3. If not found, query goes to the **recursive DNS resolver**.
  4. Resolver asks **root DNS server** → **TLD DNS server** (e.g., `.com`) → **Authoritative DNS server**.
  5. The authoritative server returns the IP address.
 
### **1. Parts of a URL Address**  
A **URL (Uniform Resource Locator)** is a web address that specifies the location of a resource on the internet. It consists of several components:

```
https://www.example.com:8080/path/page.html?query=abc#section
```

#### **Key Parts of a URL:**
1. **Protocol (Scheme)** – `https://`
   - Defines how data is transmitted (e.g., `http`, `https`, `ftp`).
   
2. **Subdomain** – `www.`
   - Optional part before the main domain (e.g., `blog.example.com`).

3. **Domain Name** – `example.com`
   - The main website address (purchased from a domain registrar).

4. **Port (Optional)** – `:8080`
   - Specifies a communication endpoint. Default ports:
     - `80` for HTTP
     - `443` for HTTPS

5. **Path** – `/path/page.html`
   - Specifies the location of a resource on the server.

6. **Query Parameters (Optional)** – `?query=abc`
   - Key-value pairs (`key=value`) used for passing data (e.g., search queries).

7. **Fragment (Anchor) (Optional)** – `#section`
   - Jumps to a specific section of the page.

---

### **2. Buying Domains from Registrars**
To have a custom domain (e.g., `mywebsite.com`), you must **purchase** it from a domain registrar.

#### **Steps to Buy a Domain:**
1. **Choose a Domain Name** – Pick a unique and memorable name.
2. **Check Availability** – Use a domain registrar’s search tool (e.g., GoDaddy, Namecheap).
3. **Select a TLD (Top-Level Domain)** – `.com`, `.org`, `.net`, `.io`, etc.
4. **Register and Pay** – Buy the domain for 1+ years.
5. **Configure DNS Settings** – Point the domain to your website’s hosting.

---

### **3. ICANN (Internet Corporation for Assigned Names and Numbers)**
- **ICANN** is a **non-profit organization** that manages domain name registration and **oversees** domain registrars.
- It **coordinates**:
  - **Domain Name System (DNS)**
  - **IP address allocation**
  - **Top-Level Domains (TLDs)**
- **Registrars** are accredited by ICANN to sell domain names.

### **1. Remote Procedure Calls (RPC)**
- **Remote Procedure Call (RPC)** allows a client to execute functions **remotely** on a server as if they were local.
- Used in **distributed systems** where processes run on different machines but communicate seamlessly.

#### **How RPC Works:**
1. **Client Calls a Function** – The client makes a request to execute a procedure on the server.
2. **Request Sent Over Network** – The RPC framework serializes the request (e.g., using JSON, Protobuf).
3. **Server Executes the Function** – The server processes the request and returns the result.
4. **Response Sent Back** – The server sends the result, which is deserialized on the client.

#### **Common RPC Frameworks:**
- **gRPC** (Google’s RPC) – Uses **Protocol Buffers (Protobuf)** for serialization.
- **JSON-RPC** – A lightweight RPC using **JSON**.
- **XML-RPC** – Uses XML for data transmission.

#### **Use Case:**
- **Microservices communication** (e.g., service-to-service calls in Kubernetes).
- **Databases** (e.g., accessing remote procedures in databases like PostgreSQL).

---

### **2. HTTP - Request-Response Protocol**
- **HTTP (Hypertext Transfer Protocol)** is a **stateless, request-response** protocol used for communication between clients and servers.
- The **client (browser or API client)** sends a request, and the **server** responds.

#### **Steps in HTTP Communication:**
1. Client sends an **HTTP request** to the server.
2. Server processes the request.
3. Server sends an **HTTP response** back to the client.

---

### **3. Anatomy of an HTTP Request**
A **request** consists of:
- **Request Line** – Method, URL, and HTTP version.
- **Headers** – Metadata (e.g., `Content-Type`, `Authorization`).
- **Body (Optional)** – Data (e.g., JSON payload in `POST`).

#### **Example Request:**
```http
POST /api/login HTTP/1.1
Host: example.com
Content-Type: application/json
Authorization: Bearer <token>

{
  "username": "user123",
  "password": "securepass"
}
```

#### **Response from Server:**
- **Status Line** – HTTP version, status code, and message.
- **Headers** – Metadata (e.g., `Content-Length`, `Set-Cookie`).
- **Body** – Response data.

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "Login successful",
  "token": "abc123xyz"
}
```

---

### **4. HTTP Request Components**
- **Methods (Verbs)** – Define the action to be performed.
  - `GET` – Retrieve data.
  - `POST` – Send data.
  - `PUT` – Update data.
  - `DELETE` – Remove data.
- **Headers** – Extra info (e.g., `User-Agent`, `Cache-Control`).
- **Body** – Data sent in `POST`/`PUT` requests.

---

### **5. API Routes & Endpoints**
- **API Route** – A URL path that maps to a server function.
  - Example: `/api/users`
- **Endpoint** – A **specific** API route with a method.
  - Example: `GET /api/users` (fetch users)
  - Example: `POST /api/users` (create user)

---

### **6. HTTP Status Codes**
- **1xx (Informational)** – Processing (e.g., `100 Continue`).
- **2xx (Success)** – Request successful.
  - `200 OK`
  - `201 Created`
- **3xx (Redirection)** – Client should follow another URL.
  - `301 Moved Permanently`
  - `304 Not Modified`
- **4xx (Client Errors)** – Request has an issue.
  - `400 Bad Request`
  - `401 Unauthorized`
  - `404 Not Found`
- **5xx (Server Errors)** – Issue on the server.
  - `500 Internal Server Error`
  - `503 Service Unavailable`

---

### **7. SSL & TLS - Secure HTTP Communication**
- **SSL (Secure Sockets Layer)** and **TLS (Transport Layer Security)** encrypt HTTP communication (`HTTPS`).
- **TLS is the modern standard**, replacing SSL.
- Uses **public-key cryptography** to secure data.
- **Example:**
  - `http://example.com` → **Not Secure**
  - `https://example.com` → **Secure (TLS/SSL used)**

#### **TLS/SSL Handshake:**
1. **Client sends a request** to the server (`Client Hello`).
2. **Server responds** with its TLS certificate.
3. **Client verifies** the certificate (via CA).
4. **Key exchange** happens (using asymmetric encryption).
5. **Secure encrypted connection established**.

## **1. FTP (File Transfer Protocol)**
- **FTP (File Transfer Protocol)** is used to **transfer files** between a client and a server.
- Operates on the **Application Layer** of the TCP/IP model.
- Works over **TCP** (ensures reliable file transfers).
- Uses **two ports**:
  - **Port 21** – Control connection (commands and responses).
  - **Port 20** – Data transfer connection (actual file transfer).

### **Modes of FTP:**
1. **Active Mode** – Server initiates the data connection.
2. **Passive Mode** – Client initiates both connections (used in firewalled networks).

### **Drawbacks of FTP:**
- **Unencrypted** by default (credentials sent in plain text).
- **Replaced by SFTP (Secure FTP) and FTPS (FTP over SSL/TLS)** for security.

---

## **2. SMTP (Simple Mail Transfer Protocol)**
- **SMTP** is used for sending emails between servers.
- Works over **TCP (Port 25, 465 for SSL, 587 for TLS)**.
- **Does not handle email retrieval** (IMAP/POP3 is used for receiving emails).

### **How SMTP Works:**
1. User sends an email via an SMTP server.
2. SMTP server forwards the email to the recipient's mail server.
3. The recipient retrieves the email using IMAP or POP3.

### **Drawbacks of SMTP:**
- No encryption by default (**can be secured with TLS**).
- Cannot retrieve emails (only sends).

---

## **3. SSH (Secure Shell)**
- **SSH (Secure Shell Protocol)** is used for secure remote access to servers.
- Works over **TCP (Port 22)**.
- **Encrypts** communication (unlike Telnet).

### **Key Features of SSH:**
- Uses **public-key cryptography** for authentication.
- Supports **port forwarding** (secure tunneling).
- Used for **remote server management**.

### **Common SSH Commands:**
- `ssh user@server.com` → Connect to a server.
- `scp file user@server.com:/path/` → Secure copy.

---

## **4. WebRTC (Web Real-Time Communication)**
- **WebRTC** is used for **real-time** audio, video, and data sharing in web applications (e.g., video calls).
- Supports **peer-to-peer** (P2P) communication.
- Uses **both TCP and UDP**.

### **Protocols Used in WebRTC:**
1. **UDP** – Used for media streams (low latency).
2. **TCP** – Used as a fallback when UDP fails.
3. **STUN/TURN Servers** – Help in NAT traversal.

### **Drawbacks of WebRTC:**
- **Firewall issues** (some networks block UDP).
- **Requires signaling** (using WebSockets, HTTP, or other methods).
- **Security concerns** (browser permissions required for mic/camera).

---

## **5. Drawbacks of HTTP**
- **Stateless** – No built-in session management (requires cookies or tokens).
- **Unencrypted by default** – Data sent in plain text (fixed by HTTPS).
- **Latency issues** – Each request requires a full TCP handshake.
- **Polling overhead** – HTTP needs repeated polling for updates (inefficient).

---

## **6. Polling (Inefficient for Real-Time Data)**
- **Polling** is a method where the client repeatedly requests the server for updates.
- Example: A client sends a request every 5 seconds to check for new messages.

### **Drawbacks of Polling:**
- **Wastes resources** – Many requests even when no new data is available.
- **High latency** – New updates are delayed until the next request.
- **Better alternatives**: WebSockets, Server-Sent Events (SSE).

---

## **7. WebSockets (Persistent Connection for Real-Time Communication)**
- **WebSockets** allow **bidirectional, full-duplex** communication between the client and server.
- Unlike HTTP, WebSockets keep a connection **open**.

### **How WebSockets Work:**
1. **Client requests an upgrade from HTTP to WebSockets:**
   ```http
   GET /chat HTTP/1.1
   Host: example.com
   Upgrade: websocket
   Connection: Upgrade
   ```
2. **Server accepts and upgrades the connection:**
   ```http
   HTTP/1.1 101 Switching Protocols
   Upgrade: websocket
   Connection: Upgrade
   ```
3. **Once upgraded**, the connection remains open, allowing real-time data transfer.

### **WebSockets vs. Polling:**
| Feature      | Polling | WebSockets |
|-------------|--------|------------|
| Connection  | New request per poll | Persistent connection |
| Latency     | High | Low |
| Efficiency  | Wasteful requests | Efficient |
| Use Case    | Simple updates | Real-time apps (chat, gaming) |

---

## **8. Trade-offs of WebSockets**
### **Pros:**
- **Low latency** (ideal for real-time applications).
- **Efficient** (reduces request overhead).
- **Full-duplex communication** (client and server can send messages anytime).

### **Cons:**
- **Not supported everywhere** (some firewalls block WebSockets).
- **More complex implementation** (compared to REST APIs).
- **Resource-intensive** (server must maintain open connections).

- 
