---
title: Microservice Architecture and Design
tags: Microservices
created: 2025-06-30T14:49:00
---
## Gateway 

- Endpoint that is exposed to other services
	- can be internet for public APIs
	- More likely to be internal
- Abstracts implementation of services
- Client calls URL, is unaware of routing taking place to running instance
- Acts as roughly a proxy for network traffic
- Can also act as a **load balancer**

## Service Instance

- Expect to be running N number of Services
- Exact number depends on reliability and load requirements
- Minimum might be 3, for high availability
- Some tools allow you to dynamically scale based on load or anticipated load
	- Think Netflix at night
	- In evenings, Netflix traffic is 1/3 of US internet traffic
	- Netflix will scale up and down with load

## Database Tier

- Typically one database per microservice
	- Guideline - not a hard rule
- Highly scalable services will often have one transactional database
	- and one or more read database (replicas)
- Organizations will often have more than one database technology
- Not uncommon to see mix of SQL and NoSQL database Technology

## Messaging

- A common pattern is to expose an API endpoint via a RESTFul API
	- dependent microservices are often message based
	- Messages follow an event or command pattern
- Messaging allows for decoupling and scalability
- Messaging can be used to define a work flow
	- New Order, Validate Order, Charge Credit Card, Allocate Inventory, Ship Order

## Downstream Services

- Often an action on a microservice will invoke actions on multiple down stream services
- For example, It is rumored a search on amazon will invoke over 100 services to return the search results - Search, Sponsors, Your History, Logging your search, etc.
- Placing a new Order might invoke the following:
	- validate order
	- pay credit card
	- allocate Inventory
	- Ship order
