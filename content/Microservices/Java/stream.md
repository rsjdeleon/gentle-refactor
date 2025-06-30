---
title: Stream API
tags:
  - Java11
---

## 🛠 Practical Examples

### 🔍 1. **Filter Active Users**

```java
List<User> activeUsers = userRepo.findAll()
.stream()
.filter(User::isActive)     
.collect(Collectors.toList());
```

---

### 🔄 2. **Map Entity to DTO**

```java
List<OrderDTO> dtos = orderRepo.findAll()
.stream()
.map(order -> new OrderDTO(order))
.collect(Collectors.toList());
```

---

### 🔁 3. **Perform Side-Effects (Logging)**

```java
orderList.stream().forEach(order -> logger.info("Order ID: " + order.getId()));
```

---

### 📥 4. **Validate with `anyMatch`**

```java
boolean hasAdmin = users.stream().anyMatch(user -> user.getRole().equals("ADMIN"));
```

---

### 🔃 5. **Sort by Date**

```java
List<Order> sorted = orders.stream() .sorted(Comparator.comparing(Order::getCreatedAt)).collect(Collectors.toList());
```
---

### 🔢 6. **Aggregate Total Price**

```java
double total = items.stream()
.map(Item::getPrice)
.reduce(0.0, Double::sum);
```

---

### 📚 7. **Convert List to Map**

```java
Map<Long, String> idToName = users.stream()
.collect(Collectors.toMap(User::getId, User::getName));
```

---

### 🔁 8. **Remove Duplicate Emails**

```java
List<String> uniqueEmails = users.stream()
.map(User::getEmail)
.distinct()
.collect(Collectors.toList());
```

---

## 📘 Summary: Stream Pipeline Pattern


```java
list.stream()     
.filter(...)       // 1. Filter if needed     
.map(...)          // 2. Transform to another form     
.sorted(...)       // 3. Optional: sort     
.limit(...)        // 4. Optional: trim result     
.collect(...)      // 5. Gather result`
```