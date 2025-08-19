---
title: Collection API
tags:
  - Collection
  - List
  - Stream
created: 2025-08-19T14:49:00
---
### 1. **List**

- **Ordered** collection, allows **duplicates**.
- Preserves **insertion order**.
- Common implementations:
    - `ArrayList` (fast random access, slow insertion/removal in middle)
    - `LinkedList` (fast insertion/removal, slower access)
- Example:
    
``` java
List<String> names = new ArrayList<>();
names.add("Raymond"); 
names.add("Raymond"); // allowed, duplicates 
System.out.println(names); // [Raymond, Raymond]
```

### 2. **Set**

- **No duplicates**, may or may not maintain order.
- Common implementations:
    - `HashSet` – no order, O(1) lookups
    - `LinkedHashSet` – maintains insertion order
    - `TreeSet` – sorted, O(log n) lookups
- Example:
    
``` java 
Set<String> roles = new HashSet<>(); 
roles.add("ADMIN"); 
roles.add("USER"); 
roles.add("ADMIN"); // ignored 
System.out.println(roles); // [ADMIN, USER]
```

### 3. **Map**

- Key–value pairs. Keys must be **unique**, values can duplicate.
- Common implementations:
    - `HashMap` – no order, O(1) lookups
    - `LinkedHashMap` – insertion order preserved
    - `TreeMap` – sorted by key
    - `ConcurrentHashMap` – thread-safe
- Example:
    
``` java 
Map<Integer, String> users = new HashMap<>(); 
users.put(1, "Alice"); 
users.put(2, "Bob"); 
users.put(1, "Charlie"); // overwrites value for key=1 
System.out.println(users); // {1=Charlie, 2=Bob}`
```

### 4. **Streams API (Java 8+)**

- Functional style processing of collections.
- Supports **filtering, mapping, reducing**.
- Example:
    
``` java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5); 
List<Integer> even = numbers.stream()
	.filter(n -> n % 2 == 0)                             
	.collect(Collectors.toList()); 
System.out.println(even); // [2, 4]`
```

### 5. **Optional**

- A container object that may or may not contain a value.
- Helps avoid `NullPointerException`.
- Example:
    
``` java
Optional<String> opt = Optional.ofNullable(null);
System.out.println(opt.orElse("default")); // default`
```

``` java
Optional<String> user = Optional.of("Raymond"); 
user.ifPresent(System.out::println); // Raymond`
```