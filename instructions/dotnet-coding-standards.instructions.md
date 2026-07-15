---
description: "Use when writing or editing C# / .NET code: classes, services, controllers, DTOs, EF Core, DI, async, error handling, and logging. Distilled from the workspace Backend Standards Library."
applyTo: "**/*.cs"
---

# .NET / C# Coding Standards

> Source of truth: `backend/.Net Coding standards.pdf`, `backend/Clean Code Standards.pdf`, `backend/Clean Code Standards for .NET 8 API Development.pdf`. These rules are **framework-version-agnostic** and apply uniformly to **.NET 8, 9, 10, and every later release** — never treat them as .NET 8-only.
>
> Cross-references: [restful-api-standards.instructions.md](restful-api-standards.instructions.md), [sql-standards.instructions.md](sql-standards.instructions.md), [dotnet-version-policy.instructions.md](dotnet-version-policy.instructions.md)

## Naming & Casing

- **PascalCase**: namespaces, classes, structs, interfaces (prefixed `I`), methods, properties, public/const fields, enums, enum values, events, exception classes.
- **camelCase**: local variables, method parameters, private fields (a leading `_` for private instance fields is allowed and preferred, e.g., `_userRepository`).
- Names reveal intent — clarity over brevity. No abbreviations (`cust`), no Hungarian/type-based prefixes (`strName`, `decimalSalary`).
- Acronyms: 2 letters upper (`IOStream`); 3+ letters PascalCase (`XmlParser`).
- Booleans use `Is`/`Has`/`Can` phrasing (`IsActive`, `HasItems`).
- Methods are verbs/verb phrases (`CalculateDiscount`); classes/properties are nouns.
- Standard suffixes: `EventHandler`, `Callback`, `Exception`, `Dictionary`, `Stream`, `Service`, `Repository`, `Controller`, `Dto`.
- Namespace format: `<Company>.<Project>[.<Feature>][.<Subnamespace>]`.

## Methods & Classes

- One method does one thing; keep methods ≤ 20–30 lines and ≤ 5 parameters (group extra params into an object/DTO).
- Apply the **Single Responsibility Principle**; no god-classes. Keep files under ~500 lines.
- Use **guard clauses / early returns**; avoid nesting deeper than 3 levels. Prefer positive conditions.
- Follow all **SOLID** principles (SRP, OCP, LSP, ISP, DIP) and the Law of Demeter.
- Prefer **immutability** (read-only properties, `init`, copy-with methods) where practical.

## Dependency Injection

- Constructor injection only; depend on **interfaces**, not concretions.
- **Never** use the service locator pattern (`IServiceProvider.GetService<T>()`) in business code.
- Register all services/repositories/`DbContext` explicitly in `Program.cs`.

## Error Handling & Nullability

- Enable **nullable reference types**; be explicit about `?`; use the null-forgiving `!` sparingly.
- Throw **specific/domain** exceptions with meaningful messages; never swallow exceptions or use empty catch blocks.
- Do not `catch (Exception)` broadly except at the centralized boundary; always log with context.
- Normalize errors via **global exception-handling middleware** returning **ProblemDetails**.

## Async, Performance & Resources

- `async`/`await` for all I/O; never block (`.Result`, `.Wait()`, `.GetAwaiter().GetResult()`).
- Use `Task.Delay`, not `Thread.Sleep`, in async paths. Flow `CancellationToken` through.
- EF Core: `AsNoTracking()` for reads, project to DTOs, avoid N+1 (`Include`/projection), paginate lists.
- Dispose resources with `using`; pick efficient data structures (e.g., `Dictionary` for O(1) lookups).
- Cache expensive read-heavy work (`IMemoryCache` / `IDistributedCache`).

## Logging & Documentation

- Structured logging with `ILogger<T>`: include operation name, correlation ID, and sanitized exception details. Never log PII/secrets/raw stack traces to clients.
- Log levels: `Information` (normal), `Warning` (recoverable), `Error` (critical).
- XML doc comments (`///`) on public APIs; comments explain **why**, not **what**. No commented-out or dead code.

## Other

- No magic numbers/strings — extract to constants, enums, or config.
- File-scoped namespaces (`namespace MyApp.Api;`).
- Organize by **feature / bounded context**, not by technical type.
- `var` only when the type is obvious; string interpolation over concatenation.
