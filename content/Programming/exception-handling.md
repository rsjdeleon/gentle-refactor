---
title: Exception handling
tags: exception
created: 2025-08-19T14:49:00
---

# ⚡ Exception Handling Best Practices in Java & Spring Boot

## 1. **Use Checked vs Unchecked Exceptions Wisely**

- **Checked exceptions**: Represent recoverable conditions (e.g., `IOException`, `SQLException`). Caller is forced to handle.
- **Unchecked exceptions** (`RuntimeException`): Represent programming errors (e.g., `NullPointerException`, `IllegalArgumentException`).
    
👉 Best practice: Use **unchecked** exceptions in business logic. Use checked exceptions only when recovery is possible.


## 2. **Never Swallow Exceptions**

❌ Bad:

``` java
try {     
	processData(); 
} catch (Exception e) {     
// ignored 
}
```
✅ Good:

``` java 
try {     
	processData(); 
} catch (Exception e) {
	log.error("Error processing data", e);
	throw new CustomAppException("Processing failed", e); 
}
```
👉 Always log or rethrow so you don’t lose debugging info.

---

## 3. **Throw Meaningful Custom Exceptions**

- Don’t just throw `Exception` or `RuntimeException`.
- Create domain-specific exceptions:

``` java
public class UserNotFoundException extends RuntimeException {
	public UserNotFoundException(String message) {
		super(message);     
	}
}
```

👉 Helps maintainers understand intent.

---

## 4. **Use `@ControllerAdvice` for Global Exception Handling (Spring Boot)**

Centralized handling → clean controllers.

``` java 
@RestControllerAdvice 
public class GlobalExceptionHandler {     

	@ExceptionHandler(UserNotFoundException.class)     
	public ResponseEntity<String> handleUserNotFound(UserNotFoundException ex) {
		return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());     
	}      
	
	@ExceptionHandler(Exception.class)     
	public ResponseEntity<String> handleGeneric(Exception ex) {
		return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
			.body("Unexpected error");     
	} 
}
```

👉 Ensures consistent error responses.

---

## 5. **Don’t Use Exceptions for Flow Control**

❌ Bad:

``` java 
try {     
	user = repo.findById(id).get(); 
} catch (NoSuchElementException e) {
	user = new User("default"); 
}
```

✅ Good:

``` java 
user = repo.findById(id).orElse(new User("default"));`
```
---

## 6. **Always Clean Up Resources**

- Use **try-with-resources** for I/O, DB connections.
    

``` java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {     
	return br.readLine(); 
} catch (IOException e) {     
	log.error("File read error", e); 
}
```

---

## 7. **Log Exceptions at the Right Level**

- **INFO**: Business warnings
- **WARN**: Non-critical issues
- **ERROR**: Failures that need attention  
    👉 Avoid logging the same exception multiple times.

---

## 8. **Preserve Stack Trace When Rethrowing**

❌ Bad:

``` java
catch (IOException e) {     
	throw new RuntimeException("Failed to process file");
} 
```

✅ Good:

``` java
catch (IOException e) {
	throw new RuntimeException("Failed to process file", e); 
}
```

👉 Always pass the cause (`e`) to preserve the root error.

---

## 9. **Return Useful Error Responses in REST APIs**

Instead of exposing stack trace → return meaningful JSON:

``` java
{   
"timestamp": "2025-08-19T10:15:30",   
"status": 404,  
"error": "Not Found",   
"message": "User with ID 123 not found",   
"path": "/api/v1/users/123"
}
```


👉 Improves client-side debugging.

---

## 10. **Define a Consistent Exception Strategy**

- Custom exceptions for business rules.
- Global exception handler for REST responses.
- Proper logging for production monitoring.
- Don’t overuse checked exceptions.