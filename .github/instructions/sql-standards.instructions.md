---
description: "Use when writing SQL: tables, columns, keys, indexes, constraints, stored procedures, functions, views, or migration scripts for SQL Server. Distilled from the workspace Backend Standards Library."
applyTo: "**/*.sql"
---

# SQL Standards (SQL Server)

> Source of truth: `backend/SQL Naming Conventions.pdf`, `backend/SQL Syntax.pdf`.
>
> Cross-references: [dotnet-coding-standards.instructions.md](dotnet-coding-standards.instructions.md), [security-standards.instructions.md](security-standards.instructions.md)

## Naming

- **PascalCase** for tables, views, columns, stored procedures, and functions. Separate multi-word names clearly and consistently; avoid quoted identifiers and whitespace in names.
- Column names are **nouns describing the field** — never a data type (`text`, `timestamp`); use full words, not vowel-stripped abbreviations.
- Avoid **reserved words** (`user`, `table`, `lock`, ...).
- **Singular** relation names (`Team`, not `Teams`).
- **Primary keys**: single-column key is `Id`; PK constraint named `PK_<Table>_<Column>` (e.g., `PK_Person_Id`), declared via `CONSTRAINT`.
- **Foreign keys**: `FK_<CurrentTable>_<Column>_<ReferencedTable>` (e.g., `FK_TeamMember_PersonId_Person`).
- **Indexes**: `IX_<Table>_<Column(s)>` (e.g., `IX_Person_FirstName_LastName`).
- **Constraints**: descriptive names (especially CHECK constraints).
- **Stored procedures**: `USP_<Object>_<Action>` (e.g., `USP_Employees_Insert`) — never the `sp_` prefix.
- **Functions**: `FN_<Action>` (e.g., `FN_CalculateTax`).
- **Views**: `VW_<Result>` (e.g., `VW_EmpOrderDetails`).

## Syntax & Documentation (stored procedures / functions / scripts)

- Begin each object with a **header block**: Author, Create date, Description.
- Maintain a **Change History** block (date, author, change).
- Document **parameters** (name — purpose — null behavior).
- Comment declarations and use **code separators** (dashed section dividers) to group logic.

```sql
-- =============================================
-- Author:      <Name>
-- Create date: <Create Date>
-- Description: <Description>
--
-- Parameters:
--   @CaseId - id of case. does not accept nulls
--
-- Change History:
--   <date> <author>: <change>
-- =============================================
```

## Security

- Use **parameterized queries exclusively** — never string-concatenate user input into SQL (guards against SQL injection).
- Use EF Core migrations for schema changes; do not rely on `EnsureCreated`/auto-sync in production.
