<!--
Sync Impact Report
Version change: template draft -> 1.0.0
Modified principles:
- Template Principle 1 -> I. Training-First, Offline-Capable Delivery
- Template Principle 2 -> II. Authorization at Every Access Boundary
- Template Principle 3 -> III. Layered Application Boundaries
- Template Principle 4 -> IV. Story-Sliced Specifications and Verification
- Template Principle 5 -> V. Consistent File and Data Workflows
Added sections:
- Operational Constraints
- Delivery Workflow
Removed sections:
- None
Templates requiring updates:
- ✅ .specify/templates/plan-template.md
- ✅ .specify/templates/spec-template.md
- ✅ .specify/templates/tasks-template.md
- ✅ README.md
Follow-up TODOs:
- None
-->

# ContosoDashboard Constitution

## Core Principles

### I. Training-First, Offline-Capable Delivery
Features MUST run locally without mandatory cloud services, paid dependencies, or internet access.
Default implementations MUST use the repository's training stack: Blazor Server, SQLite, local
filesystem storage, and mock authentication. Any infrastructure that may later move to Azure or
another hosted platform MUST be introduced behind an explicit interface and swapped through
dependency injection rather than through business-logic forks. Rationale: the project exists to
teach spec-driven delivery in a reproducible offline environment while preserving a migration path.

### II. Authorization at Every Access Boundary
Protected UI routes MUST be backed by service-layer authorization checks. Page-level protection is
necessary but not sufficient. Any read, mutation, upload, download, share action, or cross-user
query MUST verify the caller's role, ownership, or project membership to prevent IDOR and
over-broad access. Rationale: the training app demonstrates defense in depth and must model
realistic authorization discipline even with mock authentication.

### III. Layered Application Boundaries
Presentation logic belongs in Pages and Shared components, orchestration and domain rules belong
in Services, persistence belongs in Data and Models, and application composition belongs in
Program.cs. New features MUST extend the existing dependency-injected service model instead of
placing business logic directly in Razor components or bypassing ApplicationDbContext patterns.
Infrastructure-dependent behavior MUST be isolated behind focused service interfaces. Rationale:
consistent layering keeps the codebase teachable, testable, and incrementally extensible.

### IV. Story-Sliced Specifications and Verification
Every feature spec MUST define independently testable user stories, acceptance scenarios, edge
cases, and measurable outcomes. Every implementation plan MUST pass a constitution check before
design proceeds. Every task list MUST stay organized by user story and name the validation needed
for that slice. Before handoff, contributors MUST run the narrowest relevant validation available
and MUST run `dotnet build` from `ContosoDashboard/ContosoDashboard` as the minimum repository-wide
check. Rationale: this repository teaches spec-driven development, so traceability from intent to
verification is non-negotiable.

### V. Consistent File and Data Workflows
State-changing features MUST define validation rules, persistence ordering, and audit expectations
before implementation. File-backed features MUST generate a unique storage path before persistence,
save the file before committing metadata, store assets outside wwwroot, and expose access only
through authorized application endpoints. Domain models MUST follow existing repository conventions
unless a spec explicitly justifies divergence, including integer primary keys where the surrounding
domain uses them and text storage where readability is the chosen tradeoff. Rationale: predictable
workflows prevent orphaned data, broken references, and unsafe file exposure.

## Operational Constraints

- The baseline application stack is ASP.NET Core 8, Blazor Server, EF Core with SQLite, and the
	current service-oriented application structure under `ContosoDashboard/`.
- Training features MUST remain usable with the local database and local filesystem; any production
	hardening gaps or cloud-only alternatives MUST be documented explicitly in the spec.
- Features involving storage, sharing, or notifications MUST specify file limits, allowed types,
	deletion behavior, authorization rules, and side effects before implementation starts.
- User-facing performance expectations that materially affect design MUST appear in either the spec
	success criteria or the implementation plan constraints.

## Delivery Workflow

- Spec artifacts MUST capture roles and permissions, offline behavior, storage strategy, and
	measurable outcomes whenever a feature touches data access, file handling, or shared state.
- Plan artifacts MUST map work to the real repository structure and identify any new interfaces,
	migrations, or integration points needed to preserve layering.
- Task artifacts MUST include explicit work for authorization, validation, and user-visible
	verification whenever the feature changes access paths or persistence behavior.
- Reviews MUST reject work that bypasses service-layer checks, leaks infrastructure concerns into
	UI components, or leaves new behavior unverifiable.
- README and other guidance files that describe development practice MUST stay aligned with this
	constitution when the rules change.

## Governance

This constitution supersedes ad hoc local practices for specification, planning, implementation,
and review in this repository.

Amendments MUST update this file and any impacted templates or guidance documents in the same
change so downstream `/speckit.*` commands inherit the revised rules immediately.

Versioning follows semantic governance rules: MAJOR for removing or redefining a principle in a
backward-incompatible way, MINOR for adding a principle or materially expanding required guidance,
and PATCH for clarifications that do not change obligations.

Compliance review is mandatory for every spec, plan, task list, and implementation handoff.
Violations MUST either be corrected before approval or documented in the plan's complexity tracking
section with an explicit justification.

**Version**: 1.0.0 | **Ratified**: 2026-06-13 | **Last Amended**: 2026-06-13
