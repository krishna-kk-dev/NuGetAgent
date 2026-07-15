---
description: "Use when designing or editing REST API endpoints, controllers, routes, or API versioning in ASP.NET Core. Distilled from the workspace Backend Standards Library."
applyTo: "**/*Controller.cs,**/Controllers/**/*.cs,**/*Endpoints*.cs"
---

# RESTful API Naming & Patterns

> Source of truth: `backend/Restful API Naming and Patterns.pdf`.
>
> Cross-references: [dotnet-coding-standards.instructions.md](dotnet-coding-standards.instructions.md), [security-standards.instructions.md](security-standards.instructions.md)

## URI Naming

- Use **nouns**, not verbs, in URIs (HTTP verbs express the action): `GET /api/v1/users/{userId}`, never `/getUsers`.
- **Plural** nouns for collections (`/users`, `/orders`); singular only for genuine singletons (`/config`).
- **lowercase** paths; **hyphens** to separate words (`/purchase-orders`), never underscores or camelCase in path segments.
- **camelCase** for query-string parameters and route parameter names (`?pageSize=20`, `{userId}`).
- Forward slashes express **hierarchy** (`/users/{userId}/orders/{orderId}`); no trailing slash; no file extensions; ASCII only, no special/encoded chars.

## Versioning

- URL segment `v1`, `v2` at the highest scope (`/api/v{version:apiVersion}/...`); major versions only. Use `Asp.Versioning`.
- Clearly mark deprecated endpoints and provide alternatives.

## Responses & Consistency

- Return correct status codes (200/201/204/400/401/403/404/409/422/500).
- Use consistent response and error shapes across every endpoint (normalize errors via **ProblemDetails**).
- Document every endpoint with **Swagger/OpenAPI** (Swashbuckle): request/response schemas, status codes via `[ProducesResponseType]`, and XML comments.
- Handle pagination and filtering efficiently on list endpoints.
- Never leak entities directly — use DTOs for request/response contracts.
