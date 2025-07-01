---
title: JSON Jackson
tags:
  - JSON
  - JSON-to-POJO
created: 2025-07-01
---
# 🟡 Jackson Cheat Sheet

Jackson is the default JSON processor in Spring Boot. It handles serialization (Java → JSON) and deserialization (JSON → Java) with rich annotation support.

---
## 📦 Basic Setup

In Spring Boot, Jackson is auto-configured with `spring-boot-starter-web`.

```xml
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
</dependency>
```

---

## 📘 Common Jackson Annotations

|Annotation|Target|Purpose|
|---|---|---|
|`@JsonProperty`|Field/Method|Rename a JSON property|
|`@JsonIgnore`|Field/Method|Skip field during serialization/deserialization|
|`@JsonInclude`|Class/Field|Include only non-null/non-empty values|
|`@JsonFormat`|Field/Method|Format for dates/enums|
|`@JsonIgnoreProperties`|Class|Ignore fields globally|
|`@JsonCreator`|Constructor|Custom constructor for deserialization|
|`@JsonValue`|Method|Serialize object using single method|
|`@JsonAnyGetter` / `@JsonAnySetter`|Method|Dynamic properties|
|`@JsonUnwrapped`|Field|Flatten nested object|
|`@JsonAlias`|Field|Accept alternate property names|
|`@JsonAutoDetect`|Class|Customize visibility of fields/methods|
---
## 🧪 Usage Examples

### ✅ Rename a Field

```java

@JsonProperty("user_id")
private Long id;

```

### ✅ Ignore Field

```java
@JsonIgnore
private String password;
```

### ✅ Include Only Non-Null

```java
@JsonInclude(JsonInclude.Include.NON_NULL)
private String nickname;
```

  

### ✅ Format Dates

```java
@JsonFormat(pattern = "yyyy-MM-dd HH:mm")
private LocalDateTime eventDate;
```
### ✅ Ignore Multiple Fields

```java
@JsonIgnoreProperties({ "internalId", "metadata" })
public class User { ... }
```

  

### ✅ Constructor Mapping

```java
@JsonCreator
public User(@JsonProperty("username") String name) {
    this.name = name;
}
```

  

### ✅ Use Enum Code for Output

```java
@JsonValue
public String getCode() {
    return code;
}
```

  

### ✅ Allow Multiple Names for One Field

```java
@JsonAlias({ "username", "user_name" })
private String name;
```

  

### ✅ Flatten Nested Object

```java
@JsonUnwrapped
private Address address;
```
---

## ⚙️ Global Jackson Config (Optional)

```yaml

spring:

  jackson:

    default-property-inclusion: non_null

    date-format: yyyy-MM-dd HH:mm:ss

    serialization:

      write-dates-as-timestamps: false

```
---

## 🔁 Manual Serialization

```java

@Autowired
private ObjectMapper objectMapper;
String json = objectMapper.writeValueAsString(user);       // Serialize
User user = objectMapper.readValue(json, User.class);      // Deserialize
```

---
## 🧠 Tips

- Use `@JsonProperty` to enforce consistent field names.
- Use `@JsonInclude` to reduce unnecessary fields in output.
- Use `@JsonIgnore` cautiously with sensitive data.
---

## 📚 Related Annotations
  
- `@JsonDeserialize`, `@JsonSerialize` → Custom serializers/deserializers
- `@JsonTypeInfo`, `@JsonSubTypes` → Polymorphic type handling
