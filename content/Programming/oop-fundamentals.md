---
title: Object Oriented Programming
tags: oop
created: 2025-08-19T14:49:00
---
# 1. Encapsulation

**Definition**: Wrapping data (fields) and behavior (methods) together, hiding internal details, and exposing controlled access via getters/setters.

✅ Protects data integrity.  
✅ Supports **data hiding**.

**Example:**

``` java
public class BankAccount {     
	private double balance; // hidden      
	
	public BankAccount(double balance) {        
		this.balance = balance;     
	}      
	
	// controlled access     
	public double getBalance() {         
		return balance;     
	}      
	
	public void deposit(double amount) {
		if (amount > 0) {
		balance += amount;         
		}     
	} 
}`
```
---

# 🔹 2. Inheritance

**Definition**: One class acquires the properties and behaviors of another (extends).  
✅ Promotes reusability.  
✅ Supports IS-A relationship.

**Example:**
``` java
class Vehicle {
	void start() {
		System.out.println("Vehicle starts"); 
	} 
}  

class Car extends Vehicle {
	void honk() {
		System.out.println("Car honks"); 
	} 
}  // Usage Car car = new Car(); car.start(); // inherited car.honk();  // own method`
```
---

# 🔹 3. Polymorphism

**Definition**: One interface, many implementations.  
✅ Allows the same method name to behave differently.  
✅ Achieved via **method overloading** (compile-time) and **method overriding** (runtime).

**Example (Overloading - Compile-time):**

``` java
class MathUtils {

	int add(int a, int b) { 
		return a + b; 
	}     
	
	double add(double a, double b) {
		return a + b; 
	} 
}
```

**Example (Overriding - Runtime):**

``` java
class Animal {
	void sound() {
	System.out.println("Animal makes sound"); 
	} 
} 
class Dog extends Animal { 
	@Override     
	void sound() { 
	System.out.println("Dog barks"); 
	} 
}
```
---

# 🔹 4. Abstraction

**Definition**: Hiding implementation details, showing only essential features.  
✅ Achieved via **abstract classes** and **interfaces**.  
✅ Supports "what to do" rather than "how to do".

**Example (Abstract Class):**

``` java
abstract class Shape {
	abstract double area(); 
}  

class Circle extends Shape {

	private double r;

	Circle(double r) { 
		this.r = r; 
	}     
	
	@Override     
	double area() {
	return Math.PI * r * r; 
	} 
}
```

**Example (Interface):**

``` java

interface Payment {
	void pay(double amount); 
}  

class CreditCardPayment implements Payment {

	public void pay(double amount) {
		System.out.println("Paid " + amount + " using credit card");     
	} 
}
```