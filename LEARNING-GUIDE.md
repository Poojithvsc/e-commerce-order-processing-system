# 🎓 COMPLETE LEARNING GUIDE - Microservice-Kafka Project

## 📍 Project Location
**D:\Tinku anna project\project 3\microservice-kafka**

---

## 🎯 WHAT THIS PROJECT TEACHES YOU

This is a **real-world production-quality** Spring Boot microservices application that demonstrates:

### **Core Technologies:**
1. ✅ **Java & Spring Boot** - Modern Java web applications
2. ✅ **Apache Kafka** - Event-driven architecture & messaging
3. ✅ **PostgreSQL** - Relational database
4. ✅ **Docker & Docker Compose** - Containerization
5. ✅ **Microservices Architecture** - How to build distributed systems
6. ✅ **Apache HTTP Server** - Reverse proxy & load balancing

### **Design Patterns & Concepts:**
- Event-Driven Architecture (Pub-Sub pattern)
- Database per Microservice pattern
- API Gateway pattern (Apache as reverse proxy)
- CQRS (Command Query Responsibility Segregation) lite
- Message serialization (JSON)
- Consumer Groups for scalability

---

## 🏗️ PROJECT ARCHITECTURE

### **The Business Scenario:**
An **E-Commerce Order Processing System** with 3 microservices:

```
                    ┌─────────────────┐
                    │  Apache httpd   │  ← Entry point (Port 8080)
                    │  (Reverse Proxy)│
                    └────────┬────────┘
                             │
            ┌────────────────┼────────────────┐
            │                │                │
            ▼                ▼                ▼
    ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
    │    ORDER     │ │   SHIPPING   │ │   INVOICING  │
    │ Microservice │ │ Microservice │ │ Microservice │
    │              │ │              │ │              │
    │ (Producer)   │ │ (Consumer)   │ │ (Consumer)   │
    └──────┬───────┘ └──────┬───────┘ └──────┬───────┘
           │                │                │
           │      ┌─────────▼────────┐       │
           └─────►│  Kafka Broker    │◄──────┘
                  │   (Topic: order) │
                  │  5 Partitions    │
                  └─────────┬────────┘
                            │
                     ┌──────▼──────┐
                     │  Zookeeper  │
                     │ (Coordinator)│
                     └─────────────┘

                  ┌─────────────────┐
                  │   PostgreSQL    │
                  ├─────────────────┤
                  │ DB: order       │
                  │ DB: shipping    │
                  │ DB: invoicing   │
                  └─────────────────┘
```

---

## 🔄 HOW IT WORKS (Data Flow)

### **Step-by-Step Process:**

1. **User creates an order** via web UI (http://localhost:8080)
   - Goes to Apache HTTP Server
   - Forwarded to ORDER microservice

2. **ORDER microservice:**
   - Saves order to PostgreSQL (`order` database)
   - Serializes order as JSON
   - **Publishes message to Kafka** (topic: `order`)
   - Returns success to user

3. **Kafka distributes the message:**
   - Message goes to one of 5 partitions
   - Kafka keeps the message until consumed

4. **SHIPPING microservice (Consumer #1):**
   - **Listens to Kafka topic** `order`
   - Receives order message
   - **Extracts only shipping data** (customer address, items to ship)
   - Saves to PostgreSQL (`shipping` database)
   - Shows shipping details in its UI

5. **INVOICING microservice (Consumer #2):**
   - **Also listens to same Kafka topic** `order`
   - Receives same order message (separate consumer group!)
   - **Extracts only invoice data** (prices, totals, customer billing)
   - Saves to PostgreSQL (`invoicing` database)
   - Shows invoice in its UI

### **Key Insight:**
- ✅ **One message, multiple consumers** - Both shipping and invoicing get the same event
- ✅ **Decoupled** - Order service doesn't know shipping/invoicing exist
- ✅ **Asynchronous** - Order confirms immediately, processing happens later
- ✅ **Scalable** - Can run multiple instances of each service

---

## 🧩 THE 3 MICROSERVICES EXPLAINED

### **1. ORDER Microservice (Kafka Producer)**

**Location:** `microservice-kafka/microservice-kafka-order/`

**Responsibility:** Create and manage orders

**What it does:**
- Web UI for creating orders
- Manages customers and items (catalog)
- **Publishes order events to Kafka**
- Uses `KafkaTemplate` to send messages

**Technologies:**
- Spring Boot (web framework)
- Spring Data JPA (database access)
- Spring Kafka (Kafka producer)
- Thymeleaf (HTML templates)
- PostgreSQL (stores orders, customers, items)

**Why this way:**
- Keeps order creation fast (doesn't wait for shipping/invoicing)
- Single responsibility: just handle orders
- Can scale independently

---

### **2. SHIPPING Microservice (Kafka Consumer)**

**Location:** `microservice-kafka/microservice-kafka-shipping/`

**Responsibility:** Handle order shipments

**What it does:**
- **Listens to Kafka for new orders**
- Extracts shipping information (address, items)
- Creates shipment records
- Web UI to view shipments

**Technologies:**
- Spring Boot
- Spring Kafka (`@KafkaListener` annotation)
- PostgreSQL (stores shipments)
- Own consumer group: `shipping-group`

**Why this way:**
- Independent from order service
- Can process shipments at its own pace
- Can be scaled (multiple instances share work via partitions)
- Failure doesn't affect order creation

---

### **3. INVOICING Microservice (Kafka Consumer)**

**Location:** `microservice-kafka/microservice-kafka-invoicing/`

**Responsibility:** Generate invoices

**What it does:**
- **Listens to Kafka for new orders**
- Extracts billing information (prices, totals)
- Creates invoice records
- Web UI to view invoices

**Technologies:**
- Spring Boot
- Spring Kafka (`@KafkaListener`)
- PostgreSQL (stores invoices)
- Own consumer group: `invoicing-group`

**Why this way:**
- Independent billing system
- Can add complex pricing logic without affecting orders
- Separate consumer group means it gets ALL messages (not shared with shipping)

---

## 🎓 WHY KAFKA? (Why not REST API?)

### **Option 1: Direct REST API Calls (Traditional)**
```
Order Service
    │
    ├──► HTTP POST → Shipping Service
    └──► HTTP POST → Invoicing Service
```

**Problems:**
- ❌ Order service must know about all consumers
- ❌ If shipping service is down, order fails
- ❌ Synchronous - order waits for all services
- ❌ Adding new service = change order code
- ❌ Tight coupling

### **Option 2: Kafka (Event-Driven)**
```
Order Service ──► Kafka ──┬──► Shipping Service
                          └──► Invoicing Service
                                (+ any future services)
```

**Benefits:**
- ✅ Order service doesn't know who consumes
- ✅ Order completes immediately
- ✅ Services can be down - messages wait
- ✅ Add new services without changing order code
- ✅ Loose coupling
- ✅ Built-in retry, persistence, scaling

**Real-World Example:**
When you place an Amazon order:
- Order confirms immediately (fast!)
- Warehouse processes shipment (async)
- Billing generates invoice (async)
- Email sends confirmation (async)
- Analytics tracks metrics (async)

All triggered by ONE order event!

---

## 📦 DOCKER CONTAINERS

### **7 Containers Run Together:**

1. **zookeeper** - Kafka coordinator (manages Kafka brokers)
2. **kafka** - Message broker (stores and distributes events)
3. **postgres** - Database (3 separate databases inside)
4. **apache** - Reverse proxy (entry point on port 8080)
5. **order** - Order microservice
6. **shipping** - Shipping microservice
7. **invoicing** - Invoicing microservice

---

## 📊 DATABASE DESIGN

### **Database Per Microservice Pattern:**

```
PostgreSQL Instance
├── order_db
│   ├── orders
│   ├── customers
│   └── items
├── shipping_db
│   ├── shipments
│   └── shipment_lines
└── invoicing_db
    ├── invoices
    └── invoice_lines
```

**Why separate databases?**
- ✅ Each service owns its data
- ✅ No direct database sharing
- ✅ Can use different DB types per service (could use MongoDB for one)
- ✅ Independent scaling
- ✅ Clear boundaries

**Data Duplication?**
- Yes! Customer data copied to shipping & invoicing
- **This is intentional:**
  - If customer changes address, old orders unchanged
  - Each service has exactly the data it needs
  - No cross-database joins

---

## 🔍 KEY CODE CONCEPTS

### **1. Kafka Producer (Order Service)**

**File:** `OrderKafkaSender.java`

```java
@Service
public class OrderKafkaSender {
    @Autowired
    private KafkaTemplate<String, Order> kafkaTemplate;

    public void send(Order order) {
        kafkaTemplate.send("order", order);  // Publish to "order" topic
    }
}
```

**What happens:**
- `KafkaTemplate` = Spring's way to send Kafka messages
- Serializes `Order` object to JSON
- Sends to topic "order"
- Returns immediately (async)

---

### **2. Kafka Consumer (Shipping Service)**

**File:** `OrderKafkaListener.java`

```java
@Component
public class OrderKafkaListener {
    @Autowired
    private ShippingService shippingService;

    @KafkaListener(topics = "order", groupId = "shipping-group")
    public void order(Delivery delivery) {
        shippingService.ship(delivery);
    }
}
```

**What happens:**
- `@KafkaListener` = Method called when message arrives
- `topics = "order"` = Listen to "order" topic
- `groupId = "shipping-group"` = Separate consumer group
- `Delivery delivery` = Automatically deserializes JSON to Delivery object (extracts only shipping fields)

---

### **3. JSON Flexible Deserialization**

**Order JSON:**
```json
{
  "orderId": 1,
  "customer": {
    "name": "John",
    "shippingAddress": "123 Main St",
    "billingAddress": "456 Elm St"
  },
  "items": [...],
  "totalPrice": 99.99
}
```

**Shipping reads:**
```java
class Delivery {
    Long orderId;
    Customer customer;  // Only reads shippingAddress
    List<Item> items;   // Only reads name, quantity
    // ignores totalPrice, billingAddress
}
```

**Invoicing reads:**
```java
class Invoice {
    Long orderId;
    Customer customer;  // Only reads billingAddress
    List<Item> items;   // Reads prices
    Double totalPrice;
    // ignores shippingAddress
}
```

**Benefit:** One message, each service extracts what it needs!

---

## 📝 CREATED: 2025-11-19
## 📂 STATUS: Ready to run and explore

---

**Next Steps:** See RUN-GUIDE.md for step-by-step instructions to start the project
