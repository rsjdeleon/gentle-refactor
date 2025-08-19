---
title: Functional Interfaces
tags:
  - Java8
  - Stream
  - Async
---

## ✅ Practical Use Cases 

### 🔍 1. **Filtering Requests or Business Objects (`Predicate`)**

```java
List<User> activeUsers = userRepo.findAll()
.stream()
.filter(user -> user.isActive())
.collect(Collectors.toList());
```

✔️ Common in service layer when only active, verified, or matching entities are needed.

---

### 🔄 2. **Mapping Entities to DTOs (`Function`)**

```java
List<OrderDTO> orders = orderRepo.findAll()
.stream()
.map(order -> new OrderDTO(order))
.collect(Collectors.toList());
```

✔️ DTO conversions for controllers or REST API responses.

---

### 🔧 3. **Trigger Side Effects (`Consumer`)**

```java 
events.forEach(event -> logger.info("Processing event: " + event));
```

✔️ Logging, auditing, or updating states.

---

### 🧰 4. **Default Fallback Values (`Supplier`)**

```java
String region = Optional.ofNullable(request.getRegion()).orElseGet(() -> configService.getDefaultRegion());
```

✔️ Good for fallback logic, circuit breaker defaults (e.g., with `Resilience4j`).

---

### ⚙️ 5. **Async Execution (`Runnable`)**

```java 
@Async 
public void sendNotification() {
    Runnable task = () -> notificationService.send();     
    new Thread(task).start(); 
}
```

✔️ Used for sending emails, notifications, background jobs.

---

### 🧮 6. **Aggregation (`BinaryOperator`)**

```java 
int totalQty = cartItems.stream()
.map(Item::getQuantity)
.reduce(0, Integer::sum);
```

✔️ Useful for reducing/combining numeric data from lists.