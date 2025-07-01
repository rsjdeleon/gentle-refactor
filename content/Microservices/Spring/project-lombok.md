---
title: Project Lombok
tags:
  - boilerplate
  - builder
  - getter
  - setter
  - constructor
created: 2025-07-01
---
# 🧾 Project Lombok Cheat Sheet

> Lombok helps you **reduce boilerplate** code like getters/setters, constructors, builders, etc.

## ✅ Setup

### Maven

``` xml
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<version>1.18.30</version>
	<scope>provided</scope>
</dependency>
```

---
## 🛠 IntelliJ Tips

- Install **Lombok Plugin**
    
- Enable annotation processing:  
    `Settings → Build, Execution, Deployment → Compiler → Annotation Processors → ✔ Enable annotation processing`

---
## 📦 Common Lombok Annotations

### 🔷 Boilerplate Eliminators

|Annotation|Description|
|---|---|
|`@Getter`|Generates getters for all fields|
|`@Setter`|Generates setters for all fields|
|`@ToString`|Generates `toString()`|
|`@EqualsAndHashCode`|Generates `equals()` and `hashCode()`|
|`@Data`|Combines `@Getter`, `@Setter`, `@ToString`, `@EqualsAndHashCode`, and `@RequiredArgsConstructor`|
|`@Value`|Immutable version of `@Data` (final fields)|

``` java
@Data
public class User {
    private String name;
    private int age;
}
```
---

### 🔷 Constructors

|Annotation|Description|
|---|---|
|`@NoArgsConstructor`|Creates no-arg constructor|
|`@AllArgsConstructor`|Creates constructor with all fields|
|`@RequiredArgsConstructor`|Creates constructor for `final` and `@NonNull` fields|

``` java
@AllArgsConstructor
@NoArgsConstructor
@RequiredArgsConstructor
public class Product {
    @NonNull private String name;
    private double price;
}
```

### 🔷 Builders

|Annotation|Description|
|---|---|
|`@Builder`|Enables fluent `builder()` pattern|
|`@Singular`|Allows collection fields to be added one-by-one|

``` java
@Builder
public class Order {
    private String id;
    private String customer;
    
    @Singular 
    private List<String> items;
}
```
---

### 🔷 Logging

|Annotation|Logger Type|
|---|---|
|`@Slf4j`|Slf4j|
|`@Log`|Java Util Logging|
|`@Log4j`|Log4j|
|`@CommonsLog`|Apache Commons|

``` java
@Slf4j
public class Service {
    public void doSomething() {
        log.info("Doing something...");
    }
}
```
---

### 🔷 Others

|Annotation|Description|
|---|---|
|`@SneakyThrows`|Avoid try-catch for checked exceptions|
|`@NonNull`|Null check and `NullPointerException` at runtime|
|`@Cleanup`|Auto-closes resources (like `try-with-resources`)|

``` java
@SneakyThrows
public void readFile() {
    @Cleanup BufferedReader reader = new BufferedReader(new FileReader("file.txt"));
    System.out.println(reader.readLine());
}
```
---



## 🔄 When to Use What?

|Want to...|Use This|
|---|---|
|Reduce boilerplate|`@Data`, `@Value`|
|Build objects fluently|`@Builder`|
|Autogenerate logs|`@Slf4j`|
|Avoid writing constructors|`@NoArgsConstructor`, `@AllArgsConstructor`|
|Validate non-null inputs|`@NonNull`|

---

## ✅ Example Class Using Multiple Annotations

``` java
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class User {
    private Long id;
    private String name;
    private String email;
}
```
