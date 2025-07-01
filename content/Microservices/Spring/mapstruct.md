---
title: MapStruct Converter
tags:
  - DTO-to-Entity
  - mapstruct
created: 2025-07-01
---

## 🧱 MapStruct

> MapStruct is a Java annotation processor that generates **type-safe, fast, compile-time mappers** for DTOs and entities — no reflection, no runtime cost.

---

## 🚀 Setup (Spring Boot + MapStruct)

### ✅ Maven

``` xml
<dependencies>
  <dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct</artifactId>
    <version>1.5.5.Final</version>
  </dependency>

  <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
  </dependency>
</dependencies>

<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.10.1</version>
      <configuration>
        <annotationProcessorPaths>
          <path>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-processor</artifactId>
            <version>1.5.5.Final</version>
          </path>
          <path>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.30</version>
          </path>
        </annotationProcessorPaths>
      </configuration>
    </plugin>
  </plugins>
</build>

```
---

## 📄 Basic Usage

### 🧱 Entity

``` java
public class User {
    private Long id;
    private String name;
    private LocalDate birthDate;
}
```

### 📦 DTO

``` java
public class UserDto {
    private Long id;
    private String name;
    private String birthDate; // formatted as string
}
```

### ⚙️ Mapper Interface

``` java
@Mapper(componentModel = "spring", uses = { DateMapper.class })
public interface UserMapper {
    UserDto toDto(User user);
    User toEntity(UserDto dto);
}
```

---

## ⏰ Custom Date Mapper

``` java
@Component
public class DateMapper {
    private static final DateTimeFormatter FORMATTER = DateTimeFormatter.ofPattern("yyyy-MM-dd");

    public String asString(LocalDate date) {
        return date != null ? date.format(FORMATTER) : null;
    }

    public LocalDate asDate(String date) {
        return date != null ? LocalDate.parse(date, FORMATTER) : null;
    }
}
```

> You need to reference this class in `@Mapper(uses = ...)`.

---

## 🧪 Cheat Sheet: Common MapStruct Features

### ✅ 1. Field Mapping

``` java
@Mapping(source = "firstName", target = "givenName") 
UserDto toDto(User user);
```

### ✅ 2. Ignore Field

``` java
@Mapping(target = "password", ignore = true) 
UserDto toDto(User user);
```

### ✅ 3. Default Value

``` java
@Mapping(target = "status", constant = "ACTIVE") 
UserDto toDto(User user);
```

### ✅ 4. Custom Method (via `uses = {}`)

``` java
@Mapper(componentModel = "spring", uses = {DateMapper.class})
```

### ✅ 5. List Mapping

``` java
List<UserDto> toDtoList(List<User> users);
```

### ✅ 6. Expression Mapping

``` java
@Mapping(target = "createdAt", expression = "java(LocalDateTime.now())")
```
---

## 📋 Summary Table

|Feature|Annotation/Setting|
|---|---|
|Spring Bean|`@Mapper(componentModel = "spring")`|
|Custom Mapper Helper|`@Mapper(uses = { DateMapper.class })`|
|Ignore field|`@Mapping(target = "field", ignore = true)`|
|Rename field|`@Mapping(source = "a", target = "b")`|
|Format / convert dates|Use helper class (`DateMapper`)|
|Use constants/defaults|`@Mapping(..., constant = "VALUE")`|
|Map List|`List<EntityDto> toDtoList(List<Entity>)`|

---

## 🛠 Example Directory Structure

```psql
com.example.app/
├── mapper/
│   ├── UserMapper.java
│   └── DateMapper.java
├── dto/
│   └── UserDto.java
├── entity/
│   └── User.java
```
---

## 💬 Pro Tip

MapStruct generates the implementation during compile time in `target/generated-sources`. You can open it to see exactly how it's mapping things!