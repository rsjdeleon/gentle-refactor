---
title: Lambda Expressions
tags:
  - Java11
---

## ✅ Syntax

`(parameters) -> expression
(parameters) -> { statements; }`

## 🔝 Common Use Cases

### 1. 🔁 forEach (Consumer)

```java
List<String> list = Arrays.asList("Alice", "Bob"); 
list.forEach(item -> System.out.println(item));
```

✔️ _Used to iterate and perform side effects like logging or printing._

---

### 2. 🔍 filter (Predicate)

```java
List<String> filtered = list.stream()
.filter(name -> name.startsWith("A"))
.collect(Collectors.toList());
```

✔️ _Used to exclude or include items based on a condition._

---

### 3. 🔄 map (Function)

```java
List<String> uppercased = list.stream()
.map(name -> name.toUpperCase())
.collect(Collectors.toList());`
```

✔️ _Used to transform elements in a stream._

---

### 4. 🔃 sort (Comparator)

```java
list.sort((a, b) -> a.compareToIgnoreCase(b));`
```

✔️ _Used for custom sorting logic._

---

### 5. ⚙️ Runnable (Thread)

```java
Runnable task = () -> System.out.println("Running in thread"); 
new Thread(task).start();
```

✔️ _Used to create simple background threads._

---

### 6. ➕ reduce (BinaryOperator)

```java
int sum = Arrays.asList(1, 2, 3, 4).stream().reduce(0, (a, b) -> a + b);
```

✔️ _Used to accumulate values (e.g. sum, product)._

---

### 7. 📌 Method Reference

```java
list.forEach(System.out::println);
```

✔️ _Shorthand for simple lambdas like `x -> System.out.println(x)`._
