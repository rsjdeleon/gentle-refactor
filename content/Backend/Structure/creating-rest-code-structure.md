---
title: Creating RESTFul Structure
tags:
  - CreatingRestFul
created: 2025-07-01
---

### **Spring Code Structure**

### 🧱 1. **Domain Layer**

📁 `domain/`

- Represents core **business objects** (entities, enums, value objects)
- Independent of frameworks (pure Java)

``` java
public class User {
    private Long id;
    private String name;
    private String email;
    // Getters, setters, maybe domain logic
}
```

### 💾 2. **Repository Layer (Persistence)**

📁 `repository/`

- Handles database operations
- Uses `JpaRepository`, `CrudRepository`, or custom DAOs

``` java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {}

```

### 🧠 3. **Service Layer (Business Logic)**

📁 `service/`

- Contains the **core business rules**
- Coordinates between the repository and controller

``` java
@Service
public class UserService {
    private final UserRepository repo;

    public UserService(UserRepository repo) {
        this.repo = repo;
    }

    public List<User> getAllUsers() {
        return repo.findAll();
    }

    public User getUserById(Long id) {
        return repo.findById(id).orElseThrow(() -> new NotFoundException("User not found"));
    }

    public User createUser(User user) {
        return repo.save(user);
    }
}

```

### 🌐 4. **Web Layer (REST Controller)**

📁 `controller/`

- Handles HTTP requests
- Maps URLs to service methods using `@RestController`
- For the creation of [Request Methods](app://obsidian.md/request-methods) and producing return and [Status Code](app://obsidian.md/http-status-codes)

``` java
@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService service;

    public UserController(UserService service) {
        this.service = service;
    }

    @GetMapping
    public List<User> getAll() {
        return service.getAllUsers();
    }

    @GetMapping("/{id}")
    public User getById(@PathVariable Long id) {
        return service.getUserById(id);
    }

    @PostMapping
    public ResponseEntity<User> create(@RequestBody User user) {
        return new ResponseEntity<>(service.createUser(user), HttpStatus.CREATED);
    }
}
```

### 🧾 5. **DTO Layer (Optional but Recommended)**

📁 `dto/`

- Used for request/response payloads to **decouple your API from internal models**

``` java
public class UserRequest {
    private String name;
    private String email;
}

public class UserResponse {
    private Long id;
    private String name;
}
```

### 🛠️ 6. **Mapper Layer (Optional)**

📁 `mapper/`

- Converts between DTOs and domain objects

``` java
@Component
public class UserMapper {
    public User toEntity(UserRequest req) {
        User user = new User();
        user.setName(req.getName());
        user.setEmail(req.getEmail());
        return user;
    }

    public UserResponse toDto(User user) {
        return new UserResponse(user.getId(), user.getName());
    }
}

```

### 🚨 7. **Exception Handling Layer**

📁 `exception/`

``` java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(NotFoundException.class)
    public ResponseEntity<String> handleNotFound(NotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
    }
}

```

## 🧭 Recommended Folder Structure

``` pgsql
com.example.app/
├── controller/
│   └── UserController.java
├── service/
│   └── UserService.java
├── repository/
│   └── UserRepository.java
├── domain/
│   └── User.java
├── dto/
│   ├── UserRequest.java
│   └── UserResponse.java
├── mapper/
│   └── UserMapper.java
├── exception/
│   └── GlobalExceptionHandler.java
└── Application.java
```