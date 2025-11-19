# Learning Progress Tracker

**Last Updated:** 2025-11-19
**Project:** E-Commerce Order Processing System (Microservices + Kafka)
**GitHub:** https://github.com/Poojithvsc/e-commerce-order-processing-system

---

## Session History

### Session 1 - 2025-11-19
**What was completed:**
- ✅ Set up and verified all 7 Docker containers running
- ✅ Created GitHub repository: `e-commerce-order-processing-system`
- ✅ Pushed all code and documentation to GitHub
- ✅ Configured git with username (Poojithvsc) and email (poojithrokz@gmail.com)
- ✅ Created PROGRESS.md for session continuity
- ✅ Set up learning documentation structure
- ✅ Reviewed technology stack overview (Spring Boot 2.3, Java 8, Kafka, PostgreSQL, Docker)
- ✅ Decided to continue with current versions to learn concepts first
- ✅ Learned how to maintain continuity across Claude sessions

**What was learned (high-level overview only):**
- Project is an e-commerce order processing system
- Uses event-driven architecture with Apache Kafka
- Three microservices: Order (producer), Shipping (consumer), Invoicing (consumer)
- Demonstrates real-world patterns used by companies like Netflix, Uber, Amazon
- Application accessible at http://localhost:8080
- Technology versions are from 2020 but architectural concepts are timeless

**Current Status:**
- All Docker containers running and healthy
- Code pushed to GitHub successfully and backed up
- Learning documentation created (PROGRESS.md, START-HERE.md, LEARNING-GUIDE.md, RUN-GUIDE.md)
- Git properly configured for future commits
- **Ready to start actual hands-on learning** (haven't opened application yet)

**Session Focus:**
- Setup and preparation phase - covering all bases before diving into actual learning

---

## Next Session - TODO

**Priority 1: Hands-on Exploration**
- [ ] Open http://localhost:8080 in browser
- [ ] Create a few test orders
- [ ] Watch orders flow to Shipping and Invoicing services
- [ ] Verify understanding of the event flow

**Priority 2: View Kafka in Action**
- [ ] View Kafka messages in real-time (instructions in RUN-GUIDE.md)
- [ ] Understand how events are published and consumed
- [ ] See message format (JSON)

**Priority 3: Code Walkthrough**
- [ ] Read OrderKafkaSender.java (how to publish to Kafka)
- [ ] Read OrderKafkaListener.java in shipping (how to consume from Kafka)
- [ ] Understand Spring Boot annotations
- [ ] Review database schemas

**Priority 4: Database Exploration**
- [ ] Connect to PostgreSQL
- [ ] View data in order_db, shipping_db, invoicing_db
- [ ] Understand "database per service" pattern

**Priority 5: Advanced Experiments**
- [ ] Scale shipping service: `docker-compose up -d --scale shipping=2`
- [ ] View logs: `docker-compose logs -f order`
- [ ] Stop and restart services to see fault tolerance

---

## Learning Goals

**Beginner Level (Current):**
- Understand what microservices are
- Understand event-driven architecture
- See Kafka in action
- Navigate Docker containers

**Intermediate Level (Next):**
- Read and understand Java/Spring Boot code
- Modify existing code
- Add new fields to data models
- Understand Kafka partitions and consumer groups

**Advanced Level (Future):**
- Create a 4th microservice (e.g., Notification service)
- Upgrade to Java 21 and Spring Boot 3.x
- Add new Kafka topics
- Implement additional design patterns

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

---

## Notes & Observations

- Technology versions are from 2020 but concepts are timeless
- Can upgrade to modern versions later as a learning exercise
- Project demonstrates production-quality architecture patterns
- Perfect for understanding distributed systems

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

**Last Session End:** Ready to start hands-on exploration
**Next Session Start:** Begin with Priority 1 - Hands-on Exploration
