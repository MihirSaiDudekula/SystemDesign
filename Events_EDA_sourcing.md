
---

### **Event-Driven Architecture (EDA)**

**Event-Driven Architecture (EDA)** is a software design pattern that centers around the generation, detection, consumption, and reaction to **events**.

An **event** is a **data point that signals a change of state** in a system. Importantly, an event does not contain instructions about what should be done—only that something *has* happened.

Example: Clicking a button, updating a database record, or completing a transaction.

#### Key Features:

* Communication between components is **asynchronous**.
* **Loose coupling**: Components don’t need to know about each other.
* Promotes **scalability** and **modularity**.

---

### **Core Components of EDA**

1. **Event Producers**:

   * Create and **publish** events.
   * Example: A payment service publishing a "PaymentCompleted" event.

2. **Event Routers**:

   * Middleware (like a message broker) that **filters**, **routes**, and **forwards** events.
   * Example: Kafka, NATS, Amazon SNS.

3. **Event Consumers**:

   * **Subscribe** to and **consume** events.
   * Act on those events to update the system state or trigger workflows.

---

### **Event** (Definition)

An **event** is a **notification that a state change has occurred** in the system. It does **not dictate** how the system should respond to it—it simply informs that "something happened".

---

### **Event-Driven Patterns**

EDA can be implemented using different **design patterns**, based on use case:

1. **Sagas**:

   * Long-running distributed transactions broken into a series of local transactions.
   * Each step publishes events to trigger the next.

2. **Publish-Subscribe (Pub/Sub)**:

   * Multiple consumers can subscribe to events without knowing about the publisher.
   * Great for **broadcasting** data.

3. **Event Sourcing**:

   * Events themselves are stored instead of just the current state.

4. **Command Query Responsibility Segregation (CQRS)**:

   * Commands write data; queries read it.
   * Often used **together** with Event Sourcing.

---

### **Advantages of EDA**

* **Loose Coupling**: Services are independent.
* **Scalable**: Components can scale independently.
* **Agile Integration**: Easy to add new consumers.
* **Reactive**: Supports real-time system behavior.

---

### **Challenges of EDA**

* **Guaranteed Delivery**: Needs acknowledgment mechanisms.
* **Error Handling**: Harder to trace and manage failures.
* **System Complexity**: Difficult to design and test.
* **Exactly-once or in-order delivery**: Requires careful coordination.

---

### **Use Cases of EDA**

* **System Logs & Monitoring**: e.g., Server logs, security logs.
* **Analytics and Metrics**: Real-time dashboards.
* **Heterogeneous System Integration**: Interfacing systems using events.
* **Fan-out processing**: One event triggers multiple independent workflows.

---

### **Event Sourcing**

**Event Sourcing** is a data storage pattern where the **sequence of events** is recorded instead of the current state.

* Each **event** represents a change that has happened to the system.
* The **event store** is append-only and immutable.
* You can **reconstruct** the system's state by **replaying** all events in order.

#### Key Concepts:

* **System of Record** = Event Log.
* Great for **audit trails**, **replayability**, and **debugging**.

##### Example:

```plaintext
UserRegistered -> EmailVerified -> UserPromotedToAdmin
```

These events are stored, and the current state is derived by replaying them.

---

### **Event Sourcing vs Event-Driven Architecture**

| Feature  | **Event Sourcing**       | **Event-Driven Architecture (EDA)** |
| -------- | ------------------------ | ----------------------------------- |
| Purpose  | State management         | System communication                |
| Events   | Are the **state**        | Are **messages** between services   |
| Storage  | Append-only event log    | Not necessarily stored              |
| Use case | Audit, replay, debugging | Loose-coupling, scalability         |

Event Sourcing is one of the **patterns** that can be used **within** EDA but is not equivalent to EDA.

---

### **Advantages of Event Sourcing**

* **Real-time reporting**: Historical state reconstruction.
* **Fail-safety**: Restore system state from events.
* **Auditability**: Full event trail for compliance.
* **Flexibility**: Arbitrary message types can be stored.

---

### **Disadvantages of Event Sourcing**

* **High-performance infrastructure** required.
* **Message schema evolution** must be carefully handled (e.g., using **schema registry**).
* Events can have **different structures**, leading to **data format drift**.

---

### **Command and Query Responsibility Segregation (CQRS)**

**CQRS** is an architectural pattern that separates **write operations (commands)** from **read operations (queries)**.

#### Definitions:

* **Command**: Instructs the system to do something (e.g., “UpdateUserInfo”). It may fail or succeed but does not return data.
* **Query**: Asks the system for information (e.g., “GetUserInfo”). It should not change the system’s state.

##### Intuition:

By separating the responsibilities, you can **optimize** read and write parts independently.

---

### **CQRS with Event Sourcing**

* Write side stores **events** instead of final state.
* Read side builds **denormalized** views (materialized views) optimized for querying.
* Enables high-performance **reads** and **scalable writes**.

#### Architecture Sketch:

```
[Command] --> [Event Store] --(Project)--> [Read Model]
         \-> [Event Handler] --> other systems
```

---

### **Advantages of CQRS**

* **Independent scaling** of read and write workloads.
* Optimized queries without complex joins.
* Better **modularity** and **maintainability**.
* Clear separation of concerns improves design.

---

### **Disadvantages of CQRS**

* **Increased complexity** in system design.
* **Eventual consistency**: The read model may lag behind writes.
* Risk of **duplicate** or **failed messages**.
* **More maintenance overhead** due to separate models and data stores.

---

### **Use Cases of CQRS**

* Systems where **read and write loads differ** significantly.
* Domains with **evolving business rules**.
* Where **auditing**, **replayability**, or **integration** with other systems is needed.
* Better **security control** over who can write vs who can read.

---
