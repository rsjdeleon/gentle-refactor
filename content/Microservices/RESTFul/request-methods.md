---
title: HTTP Request Methods
tags: RestFul
created: 2025-06-30T14:49:00
---

# 📄 HTTP Methods Cheat Sheet

| Method   | Request Body | Response Body | Safe   | Idempotent | Cacheable | Typical Use                         |
|----------|--------------|----------------|--------|-------------|------------|--------------------------------------|
| **GET**     | ❌ No        | ✅ Yes         | ✅ Yes | ✅ Yes      | ✅ Yes     | Retrieve data (read-only)           |
| **HEAD**    | ❌ No        | ❌ No          | ✅ Yes | ✅ Yes      | ✅ Yes     | Same as GET, no body (headers only) |
| **POST**    | ✅ Yes       | ✅ Yes         | ❌ No  | ❌ No       | ❌ No*     | Submit data, create resource        |
| **PUT**     | ✅ Yes       | ✅ Yes         | ❌ No  | ✅ Yes      | ❌ No      | Replace or update a resource        |
| **PATCH**   | ✅ Yes       | ✅ Yes         | ❌ No  | ✅ Yes      | ❌ No      | Partial update to a resource        |
| **DELETE**  | ❌/✅ Optional | ✅ Yes       | ❌ No  | ✅ Yes      | ❌ No      | Delete a resource                   |
| **OPTIONS** | ❌ No        | ✅ Yes         | ✅ Yes | ✅ Yes      | ❌ No      | Discover supported methods (CORS)   |
| **TRACE**   | ❌ No        | ✅ Yes (echo)  | ✅ Yes | ✅ Yes      | ❌ No      | Diagnostic/debugging (echo request) |

---

## 🔑 Terms Glossary

- **Request Body**: Does the method typically send data to the server?
- **Response Body**: Does the server typically return data?
- **Safe**: The method does not modify server state.
- **Idempotent**: Repeating the request has the same result.
- **Cacheable**: Can the result be cached by browsers or proxies?

\* `POST` is technically cacheable if proper headers are present, but this is rare in practice.

---

## ✅ Quick Rules of Thumb

- Use `GET` to **read**, `POST` to **create**, `PUT/PATCH` to **update**, `DELETE` to **remove**.
- Use **idempotent** methods when designing retry-safe APIs.
- Avoid `GET` with request bodies — not standard-compliant.
- Always handle CORS with `OPTIONS` for preflight requests.