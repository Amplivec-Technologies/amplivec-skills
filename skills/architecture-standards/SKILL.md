---
name: architecture-standards
description: Apply Amplivec architecture conventions when designing systems, creating projects or modules, defining dependencies, placing responsibilities, refactoring architecture, or reviewing cross-layer changes. Use the generic ecosystem principles by default and apply product-specific architecture rules when they are defined.
---

# Amplivec Architecture Standards

Apply these conventions whenever designing, creating, modifying, refactoring, or reviewing the architecture of an Amplivec system.

Amplivec does not require every product to use the same technology stack or architectural style.

Different products may use different:

- Programming languages.
- Frameworks.
- Persistence technologies.
- Communication protocols.
- User interface technologies.
- Deployment models.
- Architectural patterns.

Architecture decisions must therefore be based primarily on responsibilities, dependency boundaries, maintainability, and separation of concerns rather than on a mandatory framework or technology.

Product-specific architecture rules take precedence when they are explicitly defined.

## General Principles

All Amplivec systems should follow these principles whenever they are applicable to the selected architecture.

### Separation of Concerns

Each architectural component must have a clearly defined responsibility.

Avoid placing unrelated responsibilities in the same module, project, layer, or component.

Examples of responsibilities that should normally remain separated include:

- Domain modeling.
- Application orchestration.
- Infrastructure concerns.
- Persistence.
- External service integrations.
- HTTP transport.
- Presentation.
- Shared contracts.
- Configuration.

### Explicit Dependencies

Dependencies between architectural components must be intentional and understandable.

A project should depend only on the components whose abstractions or implementations it actually requires.

Do not introduce dependencies solely for convenience.

Avoid circular dependencies.

When adding a new project reference or module dependency, verify that the dependency direction is consistent with the architecture of the product.

### Dependency Direction

Dependencies should follow the dependency graph defined by the product architecture.

Do not assume that all Amplivec products use the same dependency direction.

Before introducing a dependency:

1. Identify the architectural role of both components.
2. Inspect the existing dependency graph.
3. Verify that the new dependency follows the established direction.
4. Avoid introducing reverse dependencies that couple lower-level responsibilities to higher-level implementation details.

### Clear Responsibility Boundaries

Code must be placed in the component that owns its responsibility.

Do not place code in a layer merely because it is technically accessible from that layer.

Examples:

- Persistence implementations belong in infrastructure-oriented components.
- UI-specific models belong in presentation-oriented components.
- Shared transport contracts belong in contract-oriented components.
- Business orchestration belongs in application-oriented components when the architecture defines such a layer.

### Technology Independence

Architectural concepts should not be unnecessarily coupled to a specific framework.

A domain model should represent the business domain rather than exist solely to satisfy a persistence framework.

Application abstractions should represent use cases and responsibilities rather than implementation technologies.

Infrastructure may depend on concrete frameworks because implementing external concerns is one of its responsibilities.

### Minimize Architectural Leakage

Implementation details should not unnecessarily leak across architectural boundaries.

Examples of details that should generally remain contained include:

- ORM-specific behavior.
- Database implementation details.
- HTTP client implementation details.
- Framework-specific dependency injection configuration.
- UI framework concerns.
- External service SDKs.

When crossing boundaries, prefer abstractions or contracts appropriate to the architecture.

### Preserve Existing Architecture

When modifying an existing product, follow its established architecture unless the task explicitly requires an architectural change.

Do not silently introduce:

- New architectural layers.
- New dependency directions.
- New architectural patterns.
- New cross-project references.
- Alternative persistence strategies.
- Alternative communication patterns.

Architectural changes should be deliberate and should consider their impact on the complete dependency graph.

## Architecture Profiles

Each Amplivec product may define its own architecture profile.

A product architecture profile should describe:

- Architectural style.
- Projects or modules.
- Responsibility of each component.
- Dependency direction.
- Communication between components.
- Persistence responsibilities.
- Presentation responsibilities.
- Shared contracts.
- Dependency injection and configuration boundaries.

When a product-specific profile exists, follow it instead of applying assumptions from another Amplivec product.

# Membriana Architecture

Membriana is a pragmatic layered ASP.NET solution with a hard client-server split between `Mvc` and `Api`.

It is not a strict Clean Architecture implementation.

That distinction matters. When reading or extending Membriana, do not assume that every request must pass through a textbook application-use-case layer or that the domain model is framework-free. The codebase is more practical than doctrinaire: it keeps meaningful project boundaries, but it also allows some responsibilities to be implemented in the most convenient layer when that is the established pattern.

The strongest verified boundaries are:

- `Mvc` is a separate frontend that talks to `Api` over HTTP.
- `Application` owns repository and service abstractions.
- `Infrastructure` owns concrete implementations and persistence.
- `Contracts` is the shared kernel for DTOs and some cross-project interfaces.

The solution contains these projects:

```text
Membriana
├── Mvc
├── Api
├── Infrastructure
├── Application
├── Domain
└── Contracts
```

## Actual Dependency Graph

This graph is important for both humans and agents because it tells you which project is allowed to know about which other project at compile time. If a change requires a new project reference, stop and verify that the new dependency matches the existing architecture rather than adding it by convenience.

Direct project references:

```text
Mvc -> Contracts

Api -> Infrastructure -> Application -> Domain -> Contracts
```

Equivalent full graph:

```text
Mvc
 |
 +----------------------> Contracts

Api
 |
 v
Infrastructure
 |
 v
Application
 |
 v
Domain
 |
 v
Contracts
```

Implications:

- `Contracts` has no project references and is the lowest-level project.
- `Domain` depends on `Contracts`.
- `Application` depends on `Domain`, not directly on `Contracts`.
- `Infrastructure` depends on `Application`.
- `Api` depends only on `Infrastructure` directly, but can use `Application`, `Domain`, and `Contracts` transitively.
- `Mvc` does not reference `Api`, `Infrastructure`, `Application`, or `Domain`.

Do not add direct references that bypass this graph unless the architecture is intentionally being changed.

## Runtime Flow

The real runtime flow is:

```text
Browser
  -> Mvc controllers
  -> Mvc typed HttpClient adapters
  -> Api controllers
  -> Application interfaces and AutoMapper profiles
  -> Infrastructure repositories/services
  -> AppDbContext / SQL Server
```

The most common data transformations are:

```text
Mvc ViewModel
  <-> Contracts DTO
  <-> Domain Entity
```

Mapping is split across two projects:

- `Mvc/Profiles`: `ViewModel <-> DTO`
- `Application/Profiles`: `DTO <-> Domain`

That split is a real project convention and should usually be preserved.

In simple terms, the frontend speaks in view models, the client-server boundary speaks in DTOs, and the backend persists domain entities. That is why the mappings are split instead of being centralized in one project.

## Client-Server Boundary

Membriana has two presentation applications with different responsibilities:

- `Mvc` is the browser-facing application.
- `Api` is the backend HTTP application.

Even though they belong to the same solution, they should be treated as separate applications that communicate over HTTP.

The intended interaction is:

```text
Browser
   |
   v
Mvc
   |
   | HTTP
   v
Api
```

This boundary is one of the clearest and most stable parts of the architecture.

Why it matters:

- It keeps browser concerns out of the backend.
- It prevents the MVC application from depending on backend implementation details.
- It makes API contracts explicit through DTOs in `Contracts`.
- It forces authorization, tenancy, and validation rules to be enforced by the backend rather than trusted to the frontend.

Correct:

```text
Mvc
  -> typed HttpClient
      -> Api
```

Incorrect:

```text
Mvc
  -> Infrastructure
```

Incorrect:

```text
Mvc
  -> Application
```

Incorrect:

```text
Mvc
  -> database
```

For humans: think of `Mvc` as a consumer of the API, not as a thin shell over backend code.

For agents: if you need backend behavior from `Mvc`, add or use an HTTP endpoint and a client adapter rather than introducing a project reference.

## Project Responsibilities

### Contracts

`Contracts` is not only transport DTOs. In Membriana it acts as a shared kernel used by both frontend/backend communication and lower backend layers.

This is easy to misunderstand if you come from a stricter layered architecture. In Membriana, `Contracts` is the place for the shapes that multiple projects need to agree on. Some of those shapes are public API payloads. Others are small shared interfaces that make generic code possible across the backend.

Verified contents:

- API DTOs such as `Contracts/Dtos/Member/*`, `Contracts/Dtos/Authentication/*`, `Contracts/Dtos/User/*`.
- Shared error payloads such as `Contracts/Dtos/Common/ErrorResponseDto.cs`.
- Shared enums such as `Contracts/Enums/MemberStatus.cs`.
- Shared cross-project interfaces such as `Contracts/Interfaces/IIdentifiable.cs` and `Contracts/Interfaces/ITenantable.cs`.

Use `Contracts` for:

- Request/response DTOs exchanged with the API.
- Shared primitive contracts needed across layers.
- Cross-layer marker interfaces that multiple projects rely on.

Do not put in `Contracts`:

- Repositories.
- Service implementations.
- Controllers.
- Razor views.
- MVC view models.
- EF persistence code.

Observed nuance:

- `Contracts` interfaces are used by `Domain` entities and generic backend infrastructure, so they are broader than public HTTP contracts.

### Domain

`Domain` contains the main entity model and domain enums/interfaces.

For humans, the easiest mental model is: if you want to understand what the system persists and what the main business nouns are, start in `Domain/Entities`.

Verified contents:

- Entities in `Domain/Entities/*`.
- Domain-only interfaces in `Domain/Interfaces/*`.
- Domain enums in `Domain/Enums/*`.

Examples:

- `AppUser`, `Organization`, `Member`, `Employee`, `Payment`, `MembershipPlan`.
- `IReferenceable`.
- `AppRole` and `Domain.Enums.PricingPlan`.

Important implementation reality:

- `Domain` is framework-aware.
- Entities use data annotations and `ValidateNever`.
- `AppUser` inherits from `IdentityUser`.
- The project references EF Core and ASP.NET Identity packages.

So treat `Domain` as the canonical entity model, but not as a persistence-agnostic or framework-free core.

In other words, `Domain` is still the source of truth for the main business entities, but it is already adapted to how the application is built with ASP.NET Identity and EF Core.

Do not put in `Domain`:

- API controllers.
- MVC controllers or views.
- Repository implementations.
- DI registration.
- `DbContext`.
- EF entity configurations.

### Application

`Application` is mainly an abstraction-and-mapping layer, not a full use-case layer.

This project is important because it defines how the rest of the backend talks about work, but it should not be read as a MediatR-style use-case layer. In the current codebase it is closer to a boundary project: it exposes interfaces and mapping rules that the API consumes and Infrastructure implements.

Verified contents:

- Repository interfaces in `Application/Repositories/*`.
- Service interfaces in `Application/Services/*`.
- AutoMapper profiles in `Application/Profiles/*`.

Examples:

- `IBaseRepository<T>`, `IMemberRepository`, `IUnitOfWork`.
- `IUserService`, `IMemberService`, `IUserManagementService`, `IPaymentService`.
- `Application/Profiles/MemberProfile.cs`, `PaymentProfile.cs`, `EmployeeProfile.cs`.

Use `Application` for:

- Interfaces consumed by `Api` and implemented by `Infrastructure`.
- DTO/entity mapping rules shared by the backend.

Do not assume `Application` owns all backend orchestration. In the current codebase:

- Some workflows go through application services.
- Generic CRUD endpoints often call repositories directly from `Api`.

Do not put in `Application`:

- Concrete repositories.
- Concrete services.
- `DbContext`.
- EF entity configurations.
- API controllers.
- MVC view models.

### Infrastructure

`Infrastructure` owns concrete implementations and technical concerns.

If `Application` describes what the backend needs, `Infrastructure` describes how those needs are fulfilled in the real system.

Verified contents:

- Repository implementations in `Infrastructure/Repositories/*`.
- Service implementations in `Infrastructure/Services/*`.
- EF Core persistence in `Infrastructure/Persistence/*`.
- DI/configuration extensions in `Infrastructure/Extensions/*`.
- Infra-specific settings DTOs and external service payloads in `Infrastructure/Dtos/*`.

Examples:

- `MemberRepository`, `PaymentRepository`, `OrganizationRepository`, `UnitOfWork`.
- `UserService`, `IdentityService`, `AccountService`, `UserManagementService`, `EmailService`.
- `AppDbContext`, `AppDbContextFactory`, `Persistence/Configurations/*`.
- `DependencyInjection.AddInfrastructure(...)`.

Persistence conventions observed in code:

- `AppDbContext` lives in `Infrastructure` and inherits `IdentityDbContext<AppUser>`.
- Entity configuration is split into `IEntityTypeConfiguration<T>` classes under `Infrastructure/Persistence/Configurations`.
- `AppDbContext.OnModelCreating` applies all configurations from the assembly.
- Stable seed data for pricing plans and ASP.NET Identity roles is defined inside `AppDbContext`.
- The project includes a migrations folder declaration and a design-time factory, but there are currently no migration files checked into `Infrastructure/Migrations`.

Repository conventions observed in code:

- `BaseRepository<T>` implements shared CRUD behavior.
- Concrete repositories override `IncludeRelations(...)` when eager loading is required.
- Custom queries stay in concrete repositories, for example `PaymentRepository.GetMonthlyIncomeAsync(...)`.

Service conventions observed in code:

- Some services are thin wrappers over repositories, such as `PaymentService`.
- Some services contain real workflow/business orchestration, such as `MemberService`, `UserManagementService`, and `AccountService`.
- External integrations live here, for example `EmailService` uses Mailtrap via RestSharp.

For humans, this is usually the best place to look when you need to answer questions such as:

- How is data actually loaded or saved?
- Where is eager loading configured?
- How is an external service called?
- Where is a transaction started?

### Api

`Api` is the backend HTTP host and transport layer.

Its main job is to receive HTTP requests, authorize them, validate transport-level concerns, invoke backend behavior, and translate the result back into HTTP responses.

Verified contents:

- Controllers in `Api/Controllers/*`.
- Tenancy filters in `Api/Filters/*`.
- HTTP helper classes in `Api/Helpers/*`.
- Startup/composition root in `Api/Program.cs`.

Examples:

- Generic CRUD base controller: `Api/Controllers/BaseController.cs`.
- Workflow controllers: `AuthenticationController`, `UsersController`, `MemberStatusesController`.

Real controller patterns:

- Generic resource controllers (`EmployeesController`, `MembershipPlansController`, `PaymentsController`, `MembersController`) inherit from `BaseController<...>`.
- Controllers return HTTP responses such as `Ok`, `CreatedAtAction`, `NoContent`, `BadRequest`, `Unauthorized`, `Forbid`, `NotFound`, and `Conflict`.
- `Api` uses AutoMapper loaded from all assemblies, which allows it to use `Application` mapping profiles.

Important implementation reality:

- `Api` does not consistently delegate all behavior to application services.
- Generic CRUD endpoints frequently call repository abstractions directly.
- More complex workflows use services and/or `IUnitOfWork`.

That means API controllers are thin in intent, but not all equally thin in implementation. Some act mostly as transport adapters, while others coordinate workflows by calling several abstractions.

Examples:

- `BaseController` uses repository abstractions directly for CRUD.
- `MembersController` uses `IMemberService` for create/update because those operations also create member status events.
- `AuthenticationController` and `UsersController` coordinate multi-step workflows using services and `IUnitOfWork`.

Use `Api` for:

- Routing.
- Authorization policies.
- HTTP status/result translation.
- Request-specific tenancy enforcement.
- Backend composition root.

Do not put in `Api`:

- Razor views.
- MVC view models.
- EF entity configuration.
- Concrete repository implementations.

### Mvc

`Mvc` is a separate ASP.NET MVC frontend.

Its role is presentation, not backend execution. It renders views, handles browser navigation, prepares UI-oriented models, and delegates business operations to the API.

Verified contents:

- MVC controllers in `Mvc/Controllers/*` and `Mvc/Areas/*/Controllers/*`.
- View models in `Mvc/ViewModels/*` and some area-specific folders such as `Mvc/Areas/Admin/ViewModels/*`.
- Typed API clients in `Mvc/Clients/*`.
- Frontend mapping profiles in `Mvc/Profiles/*`.
- Cookie/JWT helpers in `Mvc/Authentication/*`.

Use `Mvc` for:

- Razor views and browser workflows.
- UI-specific models.
- Calling `Api` through typed `HttpClient` adapters.
- Translating API DTOs to UI-specific models.

Verified communication boundary:

```text
Browser
  -> Mvc controller
  -> Mvc client
  -> HTTP
  -> Api
```

`Mvc` should continue to avoid direct references to:

- `Api`.
- `Infrastructure`.
- `Application`.
- `Domain`.
- The database.

Observed frontend conventions:

- Controllers return views and redirects, not backend API-style resource responses.
- Controllers typically obtain `OrganizationId` through `IUserClient` and pass it to the API.
- Clients read/write `Contracts` DTOs and map them to/from `Mvc` view models.
- `JwtCookieHandler` forwards the `jwt` cookie as a bearer token on outbound API calls.

From a human perspective, `Mvc` is best understood as a server-rendered frontend over an HTTP API. From an agent perspective, that means UI changes often require adjusting view models, MVC client adapters, and API payload usage together.

## Where Things Belong In Membriana

Use these placements unless there is a verified existing exception.

This section is intentionally redundant with the project descriptions above. It is meant to be a quick placement guide when someone already understands the architecture and only needs to decide where a new type should live.

```text
Contracts
  DTOs
  Shared API payloads
  Shared enums
  Shared marker interfaces

Domain
  Entities
  Domain enums
  Domain-only interfaces

Application
  Repository interfaces
  Service interfaces
  DTO <-> Domain AutoMapper profiles

Infrastructure
  Repository implementations
  Service implementations
  DbContext
  EF configurations
  Design-time DbContext factory
  DI registration extensions
  Infra configuration helpers
  External integration code

Api
  API controllers
  Filters
  HTTP helper classes
  Backend startup/composition

Mvc
  MVC controllers
  Razor views
  View models
  Typed API clients
  ViewModel <-> DTO AutoMapper profiles
  Browser auth/cookie helpers
```

## Mapping Rules

Mappings are intentionally split by boundary.

This is one of the most useful conventions to preserve because it keeps each translation close to the boundary that needs it.

Use this pattern:

```text
Mvc ViewModel <-> Contracts DTO      in Mvc/Profiles
Contracts DTO <-> Domain Entity      in Application/Profiles
```

Examples:

- `Mvc/Profiles/MemberProfile.cs` maps `MemberViewModel <-> MemberCreateDto/MemberReadDto/MemberUpdateDto`.
- `Application/Profiles/MemberProfile.cs` maps `MemberCreateDto/MemberUpdateDto <-> Member` and `Member -> MemberReadDto`.

Do not collapse view models into `Contracts` just because the API returns similar fields.

Likewise, do not move DTO-to-entity mappings into `Mvc`, because that would make the frontend depend conceptually on backend persistence models.

## ViewModel And DTO Design

View models and DTOs represent different boundaries and must be shaped for their own consumers. Do not mirror domain entities or EF relationships mechanically.

### ViewModels

A view model represents the state and interactions required by a browser view.

Use composition when the UI displays or edits a related concept as a structured object:

```csharp
public ImageViewModel? ProfileImage { get; set; }
```

Do not duplicate a composed value as a flattened property in the same view model without a separate UI need. For example, avoid exposing both `ProfileImage.Url` and `ProfileImageUrl` when both represent the same input.

Expose a related entity ID alongside its composed view model only when the UI needs the ID independently, typically to select or replace an existing shared resource:

```csharp
public int MembershipPlanId { get; set; }
public MembershipPlanViewModel? MembershipPlan { get; set; }
```

In this example:

- `MembershipPlanId` binds the selected value of a form control such as a `<select>`.
- `MembershipPlan` provides the related data needed for display.
- The plan is an independently existing, shared resource selected by reference.

Do not expose the related ID merely because the domain entity or database has a foreign key. If the UI edits a child concept directly and the backend owns its lifecycle, composition is normally sufficient:

```csharp
public ImageViewModel? ProfileImage { get; set; }
```

An image ID would be justified in the view model only if the UI had a concrete interaction that used it, such as selecting an existing image from a media library or addressing the image as an independent resource.

Additional view model rules:

- Keep validation and display metadata aligned with the browser interaction.
- Nullable composition represents an optional related concept.
- Do not use `virtual` on view model properties unless a presentation framework requires a verified behavior from it; MVC view models do not use EF lazy loading.
- If one model accumulates conflicting requirements for list, details, create, and edit screens, split it into operation-specific view models instead of adding duplicate convenience properties.

### DTOs

A DTO represents an HTTP operation, not the complete UI state and not the persistence model.

Prefer operation-specific DTOs because read and write operations commonly require different shapes:

- Read DTOs may contain nested DTOs when consumers need enriched related data.
- Create and update DTOs should contain only values accepted by that operation.
- Use scalar IDs in write DTOs when the operation associates an independently existing resource.
- Use direct or flattened editable values when the backend owns the lifecycle of a composed child resource.

For example, a member read response may contain a nested image:

```csharp
public ImageReadDto? ProfileImage { get; set; }
```

Its update request may accept only the editable URL:

```csharp
public string? ProfileImageUrl { get; set; }
```

The mapping at the MVC boundary translates `ViewModel.ProfileImage.Url` to `UpdateDto.ProfileImageUrl`. The backend then creates, updates, removes, or associates the domain `Image` according to the use case.

This distinction prevents transport contracts from exposing persistence identifiers that the client does not need and avoids allowing callers to associate arbitrary child records by ID.

### Relationship Decision Guide

Before adding both a related ID and a related object, answer these questions:

1. Is the related object an independently existing resource selected by the user?
2. Does the form need its ID for a control value, route, command, or explicit association?
3. Does the UI also need the related object's descriptive data?
4. Is the child instead edited as part of its owner, with lifecycle managed by the backend?

Use the following outcomes:

- Existing shared resource selected by reference: use the ID for writes and the composed object for reads when both UI needs exist.
- Owner-managed optional child edited directly: use composition in the view model and accept only editable values in the write DTO.
- Independently addressable child: expose its ID only when the use case actually addresses or selects it.
- Different screens need substantially different shapes: define separate view models or DTOs rather than forcing one model to serve every operation.

IDs in DTOs or view models are never an authorization mechanism. The API must still verify tenant ownership and whether the referenced resource may be associated with the target entity.

## Multi-Tenancy

Multi-tenancy is a real architectural concern throughout the backend.

In practice, this means most business data belongs to an organization, and the application repeatedly checks that the logged-in user can only act inside that organization.

Verified model:

- Tenant identity is represented by `OrganizationId`.
- Tenant-aware entities and DTOs commonly implement `ITenantable` or expose `OrganizationId`.
- Logged-in user organization resolution lives in `IUserService` / `UserService`.
- JWT tokens include an `OrganizationId` claim.

Enforcement is primarily request-level, not global-query-level.

Verified enforcement points:

- `TenancyQueryFilter` validates `organizationId` query parameters.
- `TenancyRouteFilter<T, R>` validates that route resources belong to the logged-in organization.
- `BaseController.Create(...)` checks that `createDto.OrganizationId` matches the logged-in tenant.
- Many MVC controllers also re-check ownership before rendering edit/detail/delete screens.

Treat this as an established pattern, but note that it is manual and duplicated.

For humans, the practical consequence is simple: when adding a new tenant-scoped endpoint, explicitly think about where the tenant check happens. Do not assume the data layer will automatically protect you.

Do not assume there is a global EF tenant filter in `AppDbContext`; there is not.

## Authentication And Authorization

Authentication and authorization are split across the two presentation applications in different ways.

Backend:

- `Api` configures ASP.NET Identity with `AppUser` and `IdentityRole`.
- `Api` uses JWT bearer authentication.
- Authorization policies are defined in `Api/Program.cs` for `Admin`, `Employee`, and `Member`.
- `IdentityService` wraps `UserManager<AppUser>` operations.
- `UserService` generates JWTs and resolves the logged-in user and organization from the current HTTP context.

Frontend:

- `Mvc` stores the JWT in a cookie named `jwt`.
- `JwtCookieHandler` forwards that cookie to `Api` as `Authorization: Bearer ...`.
- `JwtAuthorizationFilter` only checks whether the cookie exists before allowing MVC routes.

Important exception:

- `Mvc` calls `UseAuthentication()` and `UseAuthorization()`, but it does not implement a full standard MVC authentication scheme in the current code.
- Access control in `Mvc` is effectively custom cookie-presence checking plus backend enforcement in `Api`.

Do not document MVC auth as if it were fully enforced by ASP.NET authorization attributes and cookie auth middleware.

For humans, a useful mental model is:

- `Api` is the real authority for identity, roles, and permissions.
- `Mvc` mainly stores and forwards the token, and blocks obviously anonymous access with a lightweight custom filter.

## Transactions And Unit Of Work

Transactions are used for multi-step workflows.

This part of the architecture is worth reading carefully because the name `UnitOfWork` suggests a stricter pattern than the implementation actually provides.

Verified transaction orchestration:

- `IUnitOfWork` exposes repositories, `IIdentityService`, and `BeginTransactionAsync` / `CommitAsync` / `RollbackAsync`.
- `Infrastructure/Repositories/UnitOfWork.cs` manages an EF Core transaction from `AppDbContext.Database.BeginTransactionAsync()`.
- Multi-step workflows such as registration, member status event creation, linked-user creation, and user deletion use `IUnitOfWork`.

Important implementation nuance:

- `BaseRepository<T>.AddAsync`, `UpdateAsync`, and `DeleteAsync` call `SaveChangesAsync()` immediately.
- `UnitOfWork.CommitAsync()` also calls `SaveChangesAsync()` before committing the transaction.

So the current unit-of-work pattern provides transaction scope, but persistence is still eager inside repositories.

Document this as an implementation characteristic, not as an idealized pure unit-of-work model.

For humans, the short version is: transactions are real, but repository methods still save immediately, so the pattern is only partially centralized.

## Established Rules vs Observed Patterns

Treat these as established project rules because they are strongly supported by the repository structure:

- `Mvc` talks to backend behavior through HTTP clients, not direct backend project references.
- Shared DTOs live in `Contracts`.
- MVC-specific view models live in `Mvc`.
- Repository and service interfaces live in `Application`.
- Repository and service implementations live in `Infrastructure`.
- `DbContext`, EF configurations, and DI registration live in `Infrastructure`.
- API controllers live in `Api`.

Treat these as recurring patterns rather than absolute rules:

- `Application` owns backend AutoMapper profiles.
- `Api` sometimes uses repository abstractions directly instead of always going through services.
- Multi-tenancy is enforced with action filters and manual checks rather than one central mechanism.
- MVC controllers frequently call `IUserClient` to fetch `OrganizationId` before calling other API endpoints.

## Exceptions And Architectural Inconsistencies

Do not normalize these into generic Amplivec rules.

Current exceptions or inconsistencies in Membriana include:

- `Domain` is not technology-agnostic; it is coupled to Identity, data annotations, and MVC validation attributes.
- `Contracts` contains not only external DTOs but also foundational interfaces used by backend internals.
- `Application` is not a pure use-case layer; it is mostly interfaces plus mapping.
- `Api` sometimes bypasses service abstractions and goes straight to repositories for CRUD.
- `BaseRepository<T>` assumes an `OrganizationId` property in `GetAllAsync(int organizationId)` even though `T` is constrained only to `IIdentifiable`.
- `Mvc` authentication is custom and lighter than the backend's actual authorization model.
- View model placement is not fully uniform because some admin view models live under `Areas/Admin/ViewModels` while others live in the root `Mvc/ViewModels` folder.
- No EF migration files are currently checked into `Infrastructure/Migrations` even though the project is structured to support them.

## Guidance For New Code In Membriana

Before adding code, first decide which boundary the code crosses.

When in doubt, decide based on responsibility rather than on convenience. The right question is usually not "where can this compile?" but "which layer should own this concern so the architecture stays understandable?"

Use this checklist:

1. If the type is exchanged over HTTP between frontend and backend, put it in `Contracts`.
2. If the type exists only to render MVC views or hold UI state, put it in `Mvc`.
3. If the type is a persisted business entity, put it in `Domain`.
4. If the type is a backend interface for repositories or services, put it in `Application`.
5. If the type is a concrete repository, service, EF configuration, integration, or DI/configuration helper, put it in `Infrastructure`.
6. If the type is an HTTP controller, API filter, or API-only transport helper, put it in `Api`.

Dependency guardrails:

- Do not make `Mvc` reference backend implementation projects.
- Do not put MVC view models into `Contracts`.
- Do not put concrete infrastructure code into `Application`.
- Do not put controllers into `Infrastructure` or `Domain`.
- Do not assume a stricter clean architecture than the codebase actually implements.

When the codebase itself is inconsistent, prefer matching the dominant pattern and document the exception rather than silently turning an anomaly into a general rule.

# Architecture Review

When reviewing an architectural change, verify:

- Whether the responsibility is located in the correct component.
- Whether new project dependencies follow the established dependency direction.
- Whether circular dependencies were introduced.
- Whether shared contracts remain independent.
- Whether infrastructure concerns leak into application or domain components.
- Whether presentation concerns leak into backend or lower layers.
- Whether MVC communicates with the backend through HTTP rather than bypassing the API.
- Whether DTOs and view models remain conceptually separated.
- Whether abstractions and implementations remain in their designated projects.
- Whether a new architectural element is genuinely necessary.
- Whether the change preserves the existing architectural model.

Distinguish architectural violations from implementation defects.

Do not classify a different architectural preference as a defect when the existing implementation is consistent with the product's defined architecture.

# New Amplivec Projects

When designing a new Amplivec product, do not automatically copy the Membriana architecture.

First evaluate the requirements of the product.

Consider factors such as:

- Product size.
- Expected lifetime.
- Number of developers.
- Deployment topology.
- Client-server requirements.
- Persistence requirements.
- Integration requirements.
- Scalability requirements.
- Testing requirements.
- Technology stack.
- Operational complexity.

Use only the architectural complexity justified by the product.

A small application does not need to reproduce Membriana's project structure solely for consistency with Membriana.

However, all new architectures should document:

- Component responsibilities.
- Dependency directions.
- External boundaries.
- Persistence ownership.
- Presentation ownership.
- Shared contract ownership.
- Communication mechanisms.

Once an architecture is established for a product, preserve it consistently unless an explicit architectural decision changes it.

# General Architecture Principles

When working on an Amplivec system:

- Respect the architecture defined by the product.
- Prefer clear responsibility boundaries.
- Keep dependencies explicit.
- Avoid circular dependencies.
- Do not introduce cross-layer dependencies for convenience.
- Keep shared contracts as independent as possible.
- Separate abstractions from concrete infrastructure where the architecture requires it.
- Keep persistence concerns in infrastructure-oriented components.
- Keep presentation concerns in presentation-oriented components.
- Keep domain models focused on the domain rather than on persistence frameworks.
- Do not bypass defined client-server boundaries.
- Do not assume Membriana's architecture applies to every Amplivec product.
- Document product-specific architectural decisions when they differ from existing Amplivec systems.
- Prefer the simplest architecture that satisfies the product's actual requirements.
