# Learning Progress Tracker

**Last Updated:** 2025-11-23
**Project:** E-Commerce Order Processing System (Microservices + Kafka)
**GitHub:** https://github.com/Poojithvsc/e-commerce-order-processing-system

---

## Session History

### Session 1 - 2025-11-19
**What was completed:**
- ✅ Set up and verified all 7 Docker containers running
- ✅ Understood project architecture and purpose
- ✅ Created GitHub repository: `e-commerce-order-processing-system`
- ✅ Pushed all code and documentation to GitHub
- ✅ Configured git with username and email
- ✅ Reviewed technology stack (Spring Boot 2.3, Java 8, Kafka, PostgreSQL, Docker)
- ✅ Decided to continue with current versions to learn concepts first

**What was learned:**
- Project is an e-commerce order processing system
- Uses event-driven architecture with Apache Kafka
- Three microservices: Order (producer), Shipping (consumer), Invoicing (consumer)
- Demonstrates real-world patterns used by Netflix, Uber, Amazon
- Application accessible at http://localhost:8080

**Current Status:**
- All containers running and healthy
- Code pushed to GitHub successfully
- Ready to start hands-on exploration

---

### Session 2 - 2025-11-23
**What was completed:**
- ✅ Fixed Docker containers issue (Kafka and Apache were stopped)
- ✅ Successfully restarted all 7 containers with docker-compose down/up
- ✅ Accessed application at http://localhost:8080
- ✅ **DEEP DIVE INTO CODE ARCHITECTURE** - Learned how to BUILD this from scratch!
- ✅ Studied OrderService.java - Kafka Producer implementation
- ✅ Studied OrderKafkaListener.java - Kafka Consumer implementation
- ✅ Analyzed application.properties configuration files
- ✅ Reviewed pom.xml dependencies
- ✅ Understood Docker Desktop interface and container monitoring

**What was learned - BUILDING MICROSERVICES:**

**1. The 7 Components:**
- Order Service (Spring Boot + Kafka Producer)
- Shipping Service (Spring Boot + Kafka Consumer)
- Invoicing Service (Spring Boot + Kafka Consumer)
- Kafka (Message broker)
- Zookeeper (Kafka coordinator)
- PostgreSQL (3 separate databases)
- Apache (Reverse proxy/gateway)

**2. How to Build a Kafka Producer:**
```java
@Service
public class OrderService {
    @Autowired
    private KafkaTemplate<String, Order> kafkaTemplate;  // THE KEY!

    public Order order(Order order) {
        Order result = orderRepository.save(order);      // Save to DB
        kafkaTemplate.send("order", key, order);         // Send to Kafka
        return result;
    }
}
```

**3. How to Build a Kafka Consumer:**
```java
@Component
public class OrderKafkaListener {
    @KafkaListener(topics = "order")  // THE MAGIC ANNOTATION!
    public void order(Shipment shipment, Acknowledgment acknowledgment) {
        shipmentService.ship(shipment);      // Process it
        acknowledgment.acknowledge();        // Confirm receipt
    }
}
```

**4. Key Configuration Patterns:**
- **Producer:** `spring.kafka.producer.value-serializer=JsonSerializer`
- **Consumer:** `spring.kafka.consumer.group-id=shipping` (CRITICAL for consumer groups!)
- **Consumer:** `spring.kafka.consumer.value-deserializer=JsonDeserializer`
- **Database per service:** Each service has its own database (dborder, dbshipping, dbinvoicing)

**5. Critical Dependencies (pom.xml):**
- `spring-kafka` - Gives you KafkaTemplate and @KafkaListener
- `spring-boot-starter-data-jpa` - Database access
- `postgresql` - PostgreSQL driver
- `spring-boot-starter-thymeleaf` - Web UI templates

**6. Event-Driven Architecture Benefits:**
- Decoupling: Services don't know about each other
- Async: Order service returns immediately
- Scalability: Can run multiple instances
- Reliability: Kafka stores messages until consumed

**Key Files Studied:**
- `OrderService.java:23-35` - The producer logic (ORDER SERVICE)
- `OrderKafkaListener.java:24-29` - The consumer logic (SHIPPING SERVICE)
- `application.properties` - Configuration for both producer and consumer
- `pom.xml` - Maven dependencies
- `docker-compose.yml` - Container orchestration

**Current Status:**
- All 7 containers running successfully
- Application accessible at http://localhost:8080
- **UNDERSTOOD HOW TO BUILD THIS FROM SCRATCH**
- Ready for hands-on testing and code tracing

---

## Next Session - TODO

**Priority 1: Hands-on Testing & Code Tracing**
- [ ] Create a test order via browser
- [ ] Watch order appear in all 3 services (Order, Shipping, Invoicing)
- [ ] View Docker logs in real-time to see Kafka messages
- [ ] Trace the complete flow: Browser → Order Service → Kafka → Consumers

**Priority 2: Understanding Data Models**
- [ ] Study Order.java entity (data structure)
- [ ] Study Shipment.java entity
- [ ] Understand JPA annotations (@Entity, @Id, @OneToMany)
- [ ] See how JSON serialization works

**Priority 3: Database Deep Dive**
- [ ] Connect to PostgreSQL using database client
- [ ] View tables in dborder, dbshipping, dbinvoicing
- [ ] Understand database-per-service pattern
- [ ] See how same order data exists in 3 different databases

**Priority 4: Spring Boot Framework Learning**
- [ ] Understand @Service, @Component, @Autowired annotations
- [ ] Study dependency injection pattern
- [ ] Learn Spring Boot auto-configuration
- [ ] Understand how KafkaTemplate is created automatically

**Priority 5: Build Your Own Mini-Project**
- [ ] Create a 4th microservice (Notification service)
- [ ] Make it consume from "order" topic
- [ ] Send email/SMS notifications for new orders
- [ ] Deploy using Docker

**Priority 6: Advanced Kafka Concepts**
- [ ] Understand consumer groups
- [ ] Learn about partitions (why 5 partitions?)
- [ ] Study offset management
- [ ] Test fault tolerance (stop/restart services)

---

## Learning Goals

**Beginner Level (COMPLETED!):**
- ✅ Understand what microservices are
- ✅ Understand event-driven architecture
- ✅ See Kafka in action
- ✅ Navigate Docker containers
- ✅ Understand producer/consumer pattern
- ✅ Know how to build a basic Kafka producer
- ✅ Know how to build a basic Kafka consumer

**Intermediate Level (In Progress):**
- 🔄 Read and understand Java/Spring Boot code (50% complete)
- [ ] Trace code execution with real orders
- [ ] Modify existing code (add fields, change logic)
- [ ] Understand Kafka partitions and consumer groups
- [ ] Connect to and query PostgreSQL databases
- [ ] Understand Spring Boot annotations deeply

**Advanced Level (Future):**
- [ ] Create a 4th microservice (e.g., Notification service)
- [ ] Implement error handling and retry logic
- [ ] Add monitoring and observability (metrics, logs, traces)
- [ ] Upgrade to Java 21 and Spring Boot 3.x
- [ ] Add new Kafka topics
- [ ] Implement additional design patterns (Circuit Breaker, Saga, etc.)

---

## Quick Start Command (for next session)

**If containers are stopped:**
```bash
cd "D:\Tinku anna project\project 3\microservice-kafka\docker"
docker-compose up -d
```

**Check container status:**
```bash
docker-compose ps
```

**Access application:**
```
http://localhost:8080
```

---

## Key Files to Reference

- `START-HERE.md` - Quick overview and getting started
- `LEARNING-GUIDE.md` - Complete architecture explanation
- `RUN-GUIDE.md` - Commands and troubleshooting
- `MY-LEARNING-REQUIREMENTS.md` - Your original learning requirements
- `PROGRESS.md` - This file (learning progress tracker)

---

## Questions to Explore in Next Session

1. How does Kafka ensure messages aren't lost?
2. What happens if Shipping service crashes while processing?
3. How does Spring Boot auto-configure Kafka?
4. What are Kafka partitions and why 5 partitions?
5. How does "database per service" pattern help scalability?
6. How does Spring automatically convert Order object to JSON?
7. What is consumer group and why is it important?
8. How does manual acknowledgment work in consumers?

## Questions ANSWERED This Session

**Q: How do microservices communicate without REST APIs?**
A: Through Kafka message broker using publish-subscribe pattern. Producer publishes to a topic, consumers subscribe to that topic.

**Q: What code do I need to send a message to Kafka?**
A: Just 3 things:
1. Inject KafkaTemplate
2. Call kafkaTemplate.send(topic, key, value)
3. Configure Kafka server in application.properties

**Q: What code do I need to receive messages from Kafka?**
A: Just 2 things:
1. Create a method with @KafkaListener(topics = "topic-name")
2. Configure consumer group-id in application.properties

**Q: How does each service have its own database?**
A: Each service configures a different database name in spring.datasource.url:
- Order: jdbc:postgresql://postgres/dborder
- Shipping: jdbc:postgresql://postgres/dbshipping
- Invoicing: jdbc:postgresql://postgres/dbinvoicing

---

## Notes & Observations

- Technology versions are from 2020 but concepts are timeless
- Can upgrade to modern versions later as a learning exercise
- Project demonstrates production-quality architecture patterns
- Perfect for understanding distributed systems
- **KEY INSIGHT:** The entire producer logic is just ONE method call: `kafkaTemplate.send()`
- **KEY INSIGHT:** The entire consumer logic is just ONE annotation: `@KafkaListener`
- **KEY INSIGHT:** Spring Boot does 90% of the work through auto-configuration
- The code is surprisingly simple - the power is in the architecture, not complex code

## How to Build This Project From Scratch - Summary

**Step 1: Create Spring Boot Projects** (use start.spring.io)
- Add dependencies: spring-kafka, spring-data-jpa, postgresql, thymeleaf

**Step 2: Create Data Models**
- Define @Entity classes (Order, Shipment, Invoice)
- Define @Repository interfaces (Spring creates implementations automatically)

**Step 3: Create Producer Service**
- Inject KafkaTemplate
- After saving to DB, call: `kafkaTemplate.send(topic, key, value)`

**Step 4: Create Consumer Service**
- Create method with @KafkaListener annotation
- Process the message and save to database

**Step 5: Configure application.properties**
- Producer: Set serializer to JsonSerializer
- Consumer: Set group-id and deserializer
- Both: Set database connection URL

**Step 6: Run Infrastructure with Docker**
- docker-compose.yml with Kafka, Zookeeper, PostgreSQL
- Run: `docker-compose up -d`

**Step 7: Run Your Services**
- `mvn spring-boot:run` for each service

**That's it!** The framework handles all the complexity.

---

## Resources & Links

- **GitHub Repository:** https://github.com/Poojithvsc/e-commerce-order-processing-system
- **Local Project:** D:\Tinku anna project\project 3\microservice-kafka
- **Application URL:** http://localhost:8080
- **Original Project:** https://github.com/kyle-lt/microservice-kafka

---

## Session Continuity Instructions

**To resume learning in a new Claude session, say:**

> "I'm learning microservices and Kafka. Read PROGRESS.md and START-HERE.md from my project at 'D:\Tinku anna project\project 3' and help me continue from where I left off."

Or simply:

> "Read PROGRESS.md and help me continue learning"

---

**Session 1 End:** Ready to start hands-on exploration
**Session 2 End:** Deep understanding of code architecture - ready for hands-on testing
**Next Session Start:** Create test orders and trace through the complete flow from browser to all 3 services
