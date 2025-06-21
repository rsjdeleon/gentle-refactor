---
title: Optional for Null Safety
tags:
  - Java
---

```java
Optional<User> userOpt = userRepository.findById(id);
String email = userOpt.map(User::getEmail).orElse("unknown@example.com");
```