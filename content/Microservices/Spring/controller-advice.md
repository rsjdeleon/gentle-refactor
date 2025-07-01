---
title: Controller Advice
tags:
  - Exception
  - Controller-Advice
  - Error-Handling
created: 2025-07-01
---

## ✅ @ControllerAdvice

`@ControllerAdvice` is a **specialized component** that lets you apply **global logic** to all `@Controller` or `@RestController` classes, such as:

- Handling exceptions globally
    
- Binding data to all responses
    
- Modifying model attributes
    

Think of it like a **global interceptor** for your REST API.

---

## 🧱 Common Use Cases

|Use Case|How It's Done|
|---|---|
|Global error handling|`@ExceptionHandler` inside `@ControllerAdvice`|
|Model attributes|`@ModelAttribute` methods|
|Request/response advice|`@InitBinder`, custom converters|

---

## 🧠 Basic Structure: Global Exception Handler

```java
@RestControllerAdvice // or just @ControllerAdvice for MVC
public class GlobalExceptionHandler {

    @ExceptionHandler(NotFoundException.class)
    public ResponseEntity<String> handleNotFound(NotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationErrors(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
            errors.put(error.getField(), error.getDefaultMessage())
        );
        return ResponseEntity.badRequest().body(errors);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleGeneric(Exception ex) {
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Unexpected error occurred");
    }
}
```
> 💡 `@RestControllerAdvice` = `@ControllerAdvice` + `@ResponseBody`

---

## 🎯 Custom Exception Example

``` java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class NotFoundException extends RuntimeException {
    public NotFoundException(String message) {
        super(message);
    }
}
```

You can now throw:

``` java
throw new NotFoundException("User not found");
```

---

## 📦 Validation Error Handling (Bonus)

Handle `@Valid` DTO errors:

``` java
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<Map<String, String>> handleValidationErrors(MethodArgumentNotValidException ex) {
    Map<String, String> errors = new HashMap<>();
    ex.getBindingResult().getFieldErrors().forEach(error ->
        errors.put(error.getField(), error.getDefaultMessage())
    );
    return ResponseEntity.badRequest().body(errors);
}
```

---

## 📋 Summary Table

|Annotation|Purpose|
|---|---|
|`@ControllerAdvice`|Apply logic to multiple controllers|
|`@RestControllerAdvice`|Same as above, returns JSON|
|`@ExceptionHandler`|Handle specific exception globally|
|`@ModelAttribute`|Inject global model attributes|
|`@InitBinder`|Customize binding/validation|