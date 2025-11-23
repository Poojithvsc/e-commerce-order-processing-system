# 🚀 HOW TO RUN THE PROJECT - STEP BY STEP

## 📍 **You Are Here:**
`D:\Tinku anna project\project 3\microservice-kafka`

---

## ✅ **Prerequisites Check:**

Before running, make sure you have:
- ✅ Docker Desktop installed and running
- ✅ At least 4GB RAM allocated to Docker
- ✅ Ports free: 8080, 2181, 9092, 5432

---

## 🎯 **OPTION 1: QUICK START (Using Pre-built Images)**

This is the **fastest way** - no compilation needed!

### **Step 1: Navigate to docker folder**
```bash
cd "D:\Tinku anna project\project 3\microservice-kafka\docker"
```

### **Step 2: Start all services**
```bash
docker-compose up -d
```

This will:
- Download 7 Docker images from Docker Hub
- Start all containers
- Takes 2-5 minutes on first run

### **Step 3: Wait for services to start**
```bash
# Wait 60 seconds, then check status
docker-compose ps
```

You should see 7 containers running:
- ✅ apache (port 8080)
- ✅ order
- ✅ shipping
- ✅ invoicing
- ✅ kafka
- ✅ zookeeper
- ✅ postgres

### **Step 4: Access the application**
Open browser: **http://localhost:8080**

You should see the home page with links to:
- Order Service
- Shipping Service
- Invoicing Service

---

## 🎯 **OPTION 2: BUILD FROM SOURCE (Learning Mode)**

This builds everything locally so you can modify code!

### **Step 1: Navigate to source code folder**
```bash
cd "D:\Tinku anna project\project 3\microservice-kafka\microservice-kafka"
```

### **Step 2: Build the project with Maven**
```bash
# On Windows:
mvnw.cmd clean package -DskipTests

# On Mac/Linux:
./mvnw clean package -DskipTests
```

This will:
- Compile all 3 microservices
- Download dependencies
- Create JAR files
- Takes 2-5 minutes

### **Step 3: Navigate to docker folder**
```bash
cd ../docker
```

### **Step 4: Modify docker-compose.yml**

You need to uncomment the `build:` lines in `docker-compose.yml`.

I can do this for you, or you can manually edit the file.

### **Step 5: Build Docker images**
```bash
docker-compose build
```

### **Step 6: Start all services**
```bash
docker-compose up -d
```

### **Step 7: Access the application**
Open browser: **http://localhost:8080**

---

## 🌐 **HOW TO USE THE APPLICATION**

### **Access Points:**

| Service | URL | Purpose |
|---------|-----|---------|
| **Home Page** | http://localhost:8080 | Main entry point |
| **Order Service** | http://localhost:8080/order | Create orders |
| **Shipping Service** | http://localhost:8080/shipping | View shipments |
| **Invoicing Service** | http://localhost:8080/invoicing | View invoices |

---

## 📋 **COMPLETE WORKFLOW DEMO:**

### **1. View Existing Data**
1. Go to http://localhost:8080/order
2. Click "List all orders" - You'll see sample orders
3. Go to http://localhost:8080/shipping
4. Click "List all shipments" - You'll see matching shipments
5. Go to http://localhost:8080/invoicing
6. Click "List all invoices" - You'll see matching invoices

### **2. Create a New Order**
1. Go to http://localhost:8080/order
2. Click "Create new order"
3. Select a customer (e.g., "Eberhard Wolff")
4. Add items to cart:
   - Click "Add item" next to any product
   - Adjust quantity
5. Click "Checkout"
6. **Order created!** Note the Order ID

### **3. Watch Kafka Process the Order**

**Wait 5-10 seconds** for Kafka to process...

Then:

**Check Shipping:**
1. Go to http://localhost:8080/shipping
2. Click "List all shipments"
3. **Your new order appears!**
4. Notice it shows:
   - Customer shipping address
   - Items to ship
   - No prices (shipping doesn't need prices!)

**Check Invoicing:**
1. Go to http://localhost:8080/invoicing
2. Click "List all invoices"
3. **Your new order appears!**
4. Notice it shows:
   - Customer billing address
   - Item prices and total
   - No shipping details (invoicing doesn't need that!)

### **🎓 What Just Happened:**

```
You created order
      ↓
Order service saved to DB
      ↓
Order service published to Kafka
      ↓
      ├──► Shipping service received event → Created shipment
      └──► Invoicing service received event → Created invoice
```

**One event, two consumers, asynchronous processing!**

---

## 🔍 **EXPLORING KAFKA (Advanced)**

### **View Kafka Messages:**

```bash
# Open a shell in Kafka container
docker exec -it kafka /bin/sh

# View all messages in "order" topic from beginning
kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic order --from-beginning

# Press Ctrl+C to stop
```

You'll see JSON messages like:
```json
{
  "id": 1,
  "customer": {
    "name": "Eberhard Wolff",
    "firstname": "Eberhard",
    "lastname": "Wolff",
    "street": "Unter den Linden",
    "city": "Berlin"
  },
  "orderLine": [
    {
      "count": 42,
      "item": {
        "name": "iPod",
        "price": 42.0
      }
    }
  ]
}
```

---

## 🧪 **TESTING SCALABILITY**

### **Scale Shipping to 2 Instances:**

```bash
docker-compose up -d --scale shipping=2
```

Now there are **2 shipping containers**!

Kafka will automatically distribute messages between them using partitions.

**Check it:**
```bash
docker-compose logs shipping
```

You'll see both instances receiving different orders!

---

## 📊 **CHECK SERVICE HEALTH**

### **View logs for any service:**

```bash
# Order service logs
docker-compose logs order

# Shipping service logs
docker-compose logs shipping

# Invoicing service logs
docker-compose logs invoicing

# Kafka logs
docker-compose logs kafka

# Follow logs in real-time
docker-compose logs -f order
```

### **Check database:**

```bash
# Connect to PostgreSQL
docker exec -it postgres psql -U dbuser -d order

# List tables
\dt

# View orders
SELECT * FROM orders;

# Exit
\q
```

---

## 🛑 **STOP THE APPLICATION**

### **Stop all containers:**
```bash
cd "D:\Tinku anna project\project 3\microservice-kafka\docker"
docker-compose down
```

### **Stop and remove all data (volumes):**
```bash
docker-compose down -v
```

---

## 🐛 **TROUBLESHOOTING**

### **Problem: Ports already in use**

**Solution:** Stop conflicting services
```bash
# Check what's using port 8080
netstat -ano | findstr :8080

# Stop Docker containers using those ports
docker-compose down
```

### **Problem: Containers not starting**

**Solution:** Check logs
```bash
docker-compose logs [service-name]

# Example:
docker-compose logs order
```

### **Problem: "Connection refused" errors**

**Solution:** Services might still be starting. Wait 60 seconds and try again.

### **Problem: Out of memory**

**Solution:** Increase Docker RAM
1. Open Docker Desktop
2. Settings → Resources → Memory
3. Set to at least 4GB
4. Apply & Restart

---

## 🎓 **WHAT TO EXPLORE NEXT**

1. **Code Reading:**
   - `microservice-kafka-order/src/main/java/` - Order service code
   - Look for `OrderKafkaSender.java` - See how messages are sent
   - Look for `OrderKafkaListener.java` in shipping - See how messages are received

2. **Modify and Test:**
   - Add a new field to orders
   - Change Kafka topic names
   - Add a 4th microservice (e.g., Notification)

3. **Database Exploration:**
   - Connect to PostgreSQL
   - See how data is duplicated across services
   - Understand why this is good!

4. **Kafka Deep Dive:**
   - View messages in Kafka
   - Understand partitions and consumer groups
   - Test with multiple instances

---

## 📚 **FILES TO READ:**

1. `LEARNING-GUIDE.md` - Complete architecture explanation
2. `MY-LEARNING-REQUIREMENTS.md` - Your learning goals
3. `HOW-TO-RUN.md` (original) - Original documentation
4. `README.md` (original) - Project overview

---

**🎉 YOU'RE READY! Run the project and start exploring!**

Created: 2025-11-19
