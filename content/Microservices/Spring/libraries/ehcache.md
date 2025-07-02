---
title: Ehcache
tags:
  - Cache
  - Improves-Performance
created: 2025-07-02
---

# 📦 Ehcache Implementation Guide (Spring Boot)

## ✅ 1. Add Maven Dependencies

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
<dependency>
    <groupId>org.ehcache</groupId>
    <artifactId>ehcache</artifactId>
    <version>3.10.8</version>
</dependency>
```

---
## 📄 2. Create `ehcache.xml`

Place this in `src/main/resources/ehcache.xml`:

```xml
<config
    xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'
    xmlns='http://www.ehcache.org/v3'
    xsi:schemaLocation="http://www.ehcache.org/v3 http://www.ehcache.org/schema/ehcache-core-3.0.xsd'>

    <cache alias="products">
        <key-type>java.lang.Long</key-type>
        <value-type>com.example.model.Product</value-type>
        <expiry>
            <ttl unit="minutes">10</ttl>
        </expiry>
        <resources>
            <heap unit="entries">100</heap>
        </resources>
    </cache>
</config>
```
---
## ⚙️ 3. Spring Boot Configuration

### application.properties

```properties
spring.cache.jcache.config=classpath:ehcache.xml
```

### Enable caching in main class

```java

@SpringBootApplication
@EnableCaching
public class MyApp {

    public static void main(String[] args) {
        SpringApplication.run(MyApp.class, args);
    }

}
```

---
  
## 🔁 4. Use Cache in Service Layer

```java

@Service
public class ProductService {

    @Cacheable(value = "products", key = "#id")
    public Product getProductById(Long id) {
        System.out.println("Fetching from DB...");
        return new Product(id, "Product-" + id); // simulate DB fetch
    }

    @CacheEvict(value = "products", key = "#id")
    public void evictCache(Long id) {
        System.out.println("Cache evicted for id: " + id);
    }

}
```

---

## 🧪 5. Testing with Controller

```java
@RestController
@RequestMapping("/products")
public class ProductController {

    @Autowired
    private ProductService productService;

    @GetMapping("/{id}")
    public Product get(@PathVariable Long id) {
        return productService.getProductById(id);
    }
  
    @DeleteMapping("/{id}")
    public void clearCache(@PathVariable Long id) {
        productService.evictCache(id);
    }

}
```

---

## 🧠 Use Cases for Ehcache + XML

| Use Case                        | Reason                                               |

|-------------------------------|------------------------------------------------------|

| Externalized Configuration     | Modify TTL/heap without touching Java code          |
| Legacy/Enterprise Setup        | Clean separation of logic and configuration          |
| Shared Across Modules          | One cache config for multiple services/modules       |
| DevOps Friendly                | Can be managed independently of developers           |

---

## 📎 Summary

| Annotation       | Purpose                                |
|------------------|----------------------------------------|
| `@EnableCaching` | Activates Spring caching mechanism     |
| `@Cacheable`     | Caches the method return value         |
| `@CacheEvict`    | Removes a value from the cache         |
| `@CachePut`      | Updates the cache explicitly           |

