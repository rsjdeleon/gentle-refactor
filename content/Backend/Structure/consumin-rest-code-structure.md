---
title: Consuming RESTFul Structure
tags:
  - ConsumingRestFul
created: 2025-07-01
---

### 1. **Domain Layer**

📁 `com.example.app.domain`

- Define the **models** (POJOs) that match the expected request/response from the external API.

```Java
public class UserResponse {
    private String id;
    private String name;
    // getters/setters
}
```

### 2. **DTO Layer (Optional)**

📁 `com.example.app.dto`

- Used if the external API format differs from your internal logic and you want to **map or abstract** it.

### 3. **Client Layer (API Adapter)**

📁 `com.example.app.client` or `adapter`

- Encapsulates `RestTemplate` logic
- Handles the actual HTTP calls to the external service

``` java
@Component
public class ExternalUserClient {
    private final RestTemplate restTemplate;

    @Value("${external.api.url}")
    private String baseUrl;

    public ExternalUserClient(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public UserResponse getUser(String userId) {
        String url = baseUrl + "/users/" + userId;
        return restTemplate.getForObject(url, UserResponse.class);
    }
}

```

### 4. **Service Layer**

📁 `com.example.app.service`

- Uses the client layer to call external APIs and apply **business rules** if needed.

```java
@Service
public class UserService {
    private final ExternalUserClient userClient;

    public UserService(ExternalUserClient userClient) {
        this.userClient = userClient;
    }

    public UserResponse getUserById(String id) {
        return userClient.getUser(id);
    }
}

```

### 5. **Web Layer (Controller)**

📁 `com.example.app.controller`

- Your REST endpoint that calls your service

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    private final UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserResponse> getUser(@PathVariable String id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }
}

```

### 6. **Config Layer**

📁 `com.example.app.config`

- Configure `RestTemplate` bean globally.

```java
@Configuration
public class RestTemplateConfig {
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

```

## 🧭 Recommended Folder Layout

```arduino
com.example.app/
├── config/
│   └── RestTemplateConfig.java
├── controller/
│   └── UserController.java
├── service/
│   └── UserService.java
├── client/
│   └── ExternalUserClient.java
├── domain/
│   └── UserResponse.java
└── dto/  (optional)

```