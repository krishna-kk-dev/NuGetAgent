---
description: "Use when creating a new .NET service, choosing a target framework, or editing project files (.csproj / global.json / Directory.Build.props). Defines which .NET versions are supported and how to select one."
applyTo: "**/*.csproj,**/global.json,**/Directory.Build.props"
---

# .NET Version Policy

Supported target frameworks: **.NET 8, .NET 9, .NET 10, and every later release** (LTS and STS).

> The workspace Clean Code standards (`backend/Clean Code Standards for .NET 8 API Development.pdf`) were authored against .NET 8 but are **framework-version-agnostic** — apply them uniformly across all supported versions.
>
> Cross-references: [dotnet-coding-standards.instructions.md](dotnet-coding-standards.instructions.md)

## Version Selection Rules

1. **Existing solution:** NEVER change the target framework. Detect the `<TargetFramework>` (or `<TargetFrameworks>`) in the `.csproj` / `Directory.Build.props` and match it exactly. Multi-targeting is preserved as-is.
2. **New service — default:** Target the **latest installed .NET SDK** (`dotnet --list-sdks`). Prefer the newest LTS when both an LTS and a newer STS are installed, unless the user explicitly requests a specific version.
3. **User-specified version:** If the requirements, ADO story, or user request name a version (e.g., ".NET 9"), honor it exactly — provided that SDK is installed.
4. **SDK not installed:** If the requested/target SDK is missing, stop and report it as a blocker rather than silently downgrading.
5. **Language features:** Use the C# language version that ships with the chosen SDK (e.g., C# 12 on .NET 8, C# 13 on .NET 9, C# 14 on .NET 10). Enable `Nullable` and `ImplicitUsings` on all new projects regardless of version.
6. **Pin when determinism is required:** Add a `global.json` to lock the SDK version for reproducible builds.

Apply every Clean Code, REST, and SQL standard identically across all supported versions.
