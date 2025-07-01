---
title: Bean Validation
tags:
  - Validation
created: 2025-07-01
---

# ✅ Bean Validation Cheat Sheet (Spring Boot / Jakarta)

## 🧱 Built-in Annotations

|Annotation|Description|Valid For|
|---|---|---|
|`@NotNull`|Value must not be `null`|Any type|
|`@NotEmpty`|Must not be null or empty|Strings, Collections|
|`@NotBlank`|Must not be null, empty, or whitespace|Strings|
|`@Size(min, max)`|Length or size constraints|Strings, Arrays, Collections|
|`@Min(value)`|Must be greater than or equal to value|Numbers|
|`@Max(value)`|Must be less than or equal to value|Numbers|
|`@Positive`|Must be greater than 0|Numbers|
|`@PositiveOrZero`|Must be >= 0|Numbers|
|`@Negative`|Must be less than 0|Numbers|
|`@NegativeOrZero`|Must be <= 0|Numbers|
|`@Email`|Must be a valid email format|Strings|
|`@Pattern(regexp = "")`|Must match the regex pattern|Strings|
|`@Past`|Must be a past date/time|Dates (e.g. `LocalDate`)|
|`@PastOrPresent`|Must be today or earlier|Dates|
|`@Future`|Must be in the future|Dates|
|`@FutureOrPresent`|Must be today or later|Dates|
|`@AssertTrue`|Must be `true`|Boolean|
|`@AssertFalse`|Must be `false`|Boolean|

---

## 📦 Sample DTO with Validations

``` java
public class UserRequest {

    @NotBlank(message = "Name is required")
    private String name;

    @Email(message = "Invalid email")
    @NotBlank
    private String email;

    @Size(min = 8, max = 20, message = "Password must be 8–20 characters")
    private String password;

    @Min(18)
    private int age;

    @Future(message = "Subscription end must be in the future")
    private LocalDate subscriptionEnd;

    @AssertTrue(message = "You must accept the terms")
    private boolean acceptedTerms;
}
```

## 🧪 Controller with Validation

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @PostMapping
    public ResponseEntity<String> createUser(@Valid @RequestBody UserRequest request) {
        return ResponseEntity.ok("User created successfully");
    }
}
```

## 🔐 Custom Validator Example (e.g., Password Match)

### 1. Create the annotation

``` java
@Constraint(validatedBy = PasswordMatchesValidator.class)
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface PasswordMatches {
    String message() default "Passwords do not match";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

### 2. Create the validator class

``` java
public class PasswordMatchesValidator implements ConstraintValidator<PasswordMatches, RegisterRequest> {
    @Override
    public boolean isValid(RegisterRequest request, ConstraintValidatorContext context) {
        return request.getPassword().equals(request.getConfirmPassword());
    }
}
```

### 3. Use it in a DTO

``` java
@PasswordMatches
public class RegisterRequest {
    @NotBlank
    private String password;

    @NotBlank
    private String confirmPassword;
}
```

## ✅ Tips

- Use `@Validated` on service/controller class if validating method params.
- Add a `BindingResult` parameter to your controller if you want manual error handling.
- Avoid using `@NotNull` on primitive types — use wrapper classes like `Integer`, `Boolean`.

