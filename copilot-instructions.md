# Enterprise Development Workspace

These instructions apply to all agents in this workspace, which serves both **Frontend (React + TypeScript)** and **Backend (.NET / C# / SQL Server)** work.

## How to Read These Instructions

- **Stack-agnostic** sections apply to every agent and stack: _Requirement Priority_, _Existing Code Reuse Policy_, _Git Standards_, _Development Workflow_.
- **Frontend-only** sections are labeled _(Frontend — React / TypeScript)_ and apply only to React/TypeScript work.
- **Backend-only** sections are labeled _(Backend — .NET / C# / SQL Server)_ and apply only to .NET/SQL work.
- When frontend and backend guidance differ, use the section matching the stack you are building.

---

## Requirement Priority

Requirements may come from:

1. HTML Prototype (Primary Source — frontend)
2. API Contract / Integration Spec (Primary Source — backend)
3. Existing Application Codebase
4. Database Schema / SSMS Design (backend)
5. ADO Story (Optional)
6. BRD Document (Optional)
7. BA Document (Optional)

Priority Order:

- **Frontend:** HTML Prototype > Existing Codebase > ADO Story > BRD > BA Document
- **Backend:** Existing Codebase > API Contract > DB Schema (SSMS) > ADO Story > BRD > BA Document

If only a primary source is available (an HTML Prototype for frontend, or an API Contract for backend):

- Analyze the primary source.
- Analyze the existing application.
- Identify reusable assets.
- Create a technical design.
- Proceed with implementation.

Additional documentation should be treated as supporting information.

---

## Existing Code Reuse Policy

Before creating any file, search for existing assets first:

- **Frontend:** components, hooks, services, API clients, Zustand stores, utility functions, validators.
- **Backend:** controllers/endpoints, services, repositories, DTOs, validators, mapping profiles, middleware/filters, EF Core entities/`DbContext`/migrations, shared utilities.

Implementation Order:

Reuse → Extend → Create New

Never create duplicate:

- **Frontend:** components, forms, tables, dialogs, hooks, services, API wrappers, utility functions.
- **Backend:** controllers, endpoints, services, repositories, entities, migrations, DTOs, validators.

---

## General Coding Standards (Frontend — React / TypeScript)

- Use React Functional Components only.
- Use TypeScript.
- Follow Single Responsibility Principle.
- Follow feature-based architecture.
- Use React Query for server state.
- Use Zustand for client state.
- Use a scoped plain `.css` file per component (BEM naming) — do not use CSS Modules.

For new project creation:

- Create the required src scaffold folders before feature coding: assets, components, features, hooks, layouts, pages, services, store, types, utils.
- If a folder is intentionally unused initially, keep it with a .gitkeep file.

For HTML-to-React implementation fidelity:

- Extract and define shared design tokens (colors, typography, spacing, radii, shadows) before page-level styles.
- Match prototype layout, spacing, component states, and responsive behavior as closely as possible.
- If exact parity cannot be achieved, document the deviation and reason in the verification output.

Never:

- Use class components.
- Use any.
- Hardcode URLs.
- Hardcode secrets.
- Directly manipulate DOM.

---

## Accessibility (Frontend — React / TypeScript)

Every interactive element must have:

- an accessible name (use visible text, aria-label, or aria-labelledby)
- visible text when practical; for icon-only controls, provide aria-label or aria-labelledby

All images require meaningful alt text.

Preserve accessibility attributes from prototypes.

---

## Security (Frontend — React / TypeScript)

Always:

- Validate inputs.
- Sanitize user-generated content.
- Use DOMPurify for rendered HTML.
- Use environment variables for secrets.

Never:

- Store secrets in source code.
- Store tokens in localStorage.
- Trust client-side validation alone.

---

## Performance (Frontend — React / TypeScript)

Always:

- Use React.memo where beneficial.
- Use useMemo for expensive computations.
- Use useCallback for event handlers.
- Lazy load large pages.
- Avoid unnecessary re-renders.

---

## Backend Standards (Backend — .NET / C# / SQL Server)

Detailed rules live in the dedicated instruction files (`dotnet-coding-standards`, `restful-api-standards`, `sql-standards`, `dotnet-version-policy`) and the NuGetAgent workflow. The essentials:

**Architecture & Coding**

- ASP.NET Core Web API + Entity Framework Core + SQL Server; REST APIs documented with Swagger/OpenAPI (Swashbuckle).
- Target .NET 8, 9, 10, or the latest installed SDK (never downgrade an existing solution). Enable nullable reference types and implicit usings.
- Follow SOLID and Single Responsibility; layer as Controller → Service → Repository/EF Core, with DTOs for external contracts.
- Constructor injection only; never use the service locator pattern. Register services explicitly in `Program.cs`.
- `async`/`await` for all I/O; never block (`.Result`, `.Wait()`); flow `CancellationToken`.
- Normalize errors via global exception-handling middleware returning `ProblemDetails`; structured logging with `ILogger<T>` and correlation IDs.

**Security**

- Validate all inputs (FluentValidation / DataAnnotations); never trust client input.
- Parameterized queries only (EF Core / Dapper) — never string-concatenate SQL.
- Never bind or return EF Core entities directly — use DTOs (mass-assignment protection).
- Secure endpoints with `[Authorize]` and policy/role-based authorization; never log or expose PII/secrets; keep secrets out of source and `appsettings` (use user-secrets / Key Vault / env).

**Performance**

- EF Core: `AsNoTracking()` for reads, project to DTOs, avoid N+1 (`Include`/projection), paginate list endpoints.
- Cache read-heavy work (`IMemoryCache` / `IDistributedCache`); dispose resources with `using`.

**Naming (summary — see instruction files)**

- C#: PascalCase for types/methods/properties, camelCase for locals/parameters (`_` for private fields).
- REST URIs: plural nouns, lowercase, hyphenated, versioned (`/api/v1/...`).
- SQL: PascalCase objects, `Id` primary keys, `PK_/FK_/IX_` constraints, `USP_/FN_/VW_` for procs/functions/views.

**Testing**

- xUnit + Moq + FluentAssertions for unit tests; `WebApplicationFactory<Program>` for integration tests; line coverage ≥ 80%.

---

## Git Standards

Branch Naming:

feature/<story-id>-<short-description>

Commit Messages:

Explain why the change exists, not only what changed.

---

## Development Workflow

1. Analyze Inputs
2. Analyze Existing Code
3. Identify Reusable Assets
4. Create Technical Design
5. Implement
6. Test
7. Review
8. Document

Never skip analysis before implementation.
