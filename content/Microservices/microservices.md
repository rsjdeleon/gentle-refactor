---
title: Microservices vs Monolithic Architecture
tags: Microservices
created: 2025-06-30T14:49:00
---
## Monolithic 

- Single Application - Code is stored together
- One Code Base - Typically will use one database
- One Build System - Code Releases are done as one big version
- Single executional program (i.e. WAR or EAR file) - Scaling is an all or nothing situation
- In Enterprise system - an application can become very big  
	- 10k of packages, classes
- if one component needs to increase scale, the whole application needs to scale

## Microservices

- Microservices small targeted services
- Each services has its own repository
- Microservices are isolated from other services
	- should not be bundle with other services when deployed
- Microservices are loosely coupled
	- when interacting with other services, should be done in a technology agnostic manner
	- ie - Restful web services - HTTP / JSON

