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

Membriana uses a layered client-server architecture implemented as multiple Visual Studio projects.

The current architectural components are:

```text
Membriana
├── Mvc
├── Api
├── Infrastructure
├── Application
├── Domain
└── Contracts
```

The projects have distinct responsibilities and a defined dependency graph.

## Dependency Graph

The architectural dependency flow is:

```text
Mvc
 |
 +------------------------> Contracts


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

`Mvc` has a direct dependency on `Contracts`.

The backend dependency chain is:

```text
Api
  -> Infrastructure
      -> Application
          -> Domain
              -> Contracts
```

Because of this dependency chain, `Api` can access types exposed transitively through its dependencies when the project system allows it, but it does not require a direct architectural dependency on `Contracts`.

Do not add a direct `Api` -> `Contracts` dependency merely for convenience if the existing dependency graph already provides the required architectural relationship.

`Contracts` must not depend on any other Membriana project.

The dependency graph must remain acyclic.

## Contracts

`Contracts` defines types shared across architectural boundaries.

It has no dependencies on other Membriana projects.

Responsibilities include:

- DTOs.
- Shared enumerations.
- Shared interfaces intended to cross architectural boundaries.
- Other transport or interoperability contracts.

Examples may include:

```text
Request DTOs
Response DTOs
Shared enums
Shared contract interfaces
```

`Contracts` must remain lightweight.

Do not place application behavior, persistence logic, infrastructure implementations, or presentation logic in `Contracts`.

Avoid introducing framework-specific dependencies unless they are strictly necessary for defining the contract.

## Domain

`Domain` defines the domain model used by the application.

It contains the classes representing the core entities and concepts with which the application operates.

These classes may also be mapped by Entity Framework, but they must not be designed exclusively around Entity Framework.

The domain model represents the application domain first.

Responsibilities include:

- Domain entities.
- Domain-related types.
- Domain state.
- Domain relationships required by the application.

`Domain` depends on `Contracts`.

Dependency direction:

```text
Domain
  -> Contracts
```

Do not place repository implementations, service implementations, migrations, HTTP concerns, Razor views, or API controllers in `Domain`.

## Application

`Application` defines the abstractions and application-level responsibilities used by the system.

Responsibilities include:

- Service interfaces.
- Repository interfaces.
- Application orchestration abstractions.
- Application-level use case contracts where appropriate.

`Application` depends on `Domain`.

Dependency direction:

```text
Application
  -> Domain
      -> Contracts
```

Application abstractions must not depend on concrete infrastructure implementations.

Do not place:

- Concrete repository implementations.
- Database migrations.
- Entity Framework infrastructure configuration.
- HTTP controllers.
- Razor views.
- MVC view models.

in `Application`.

## Infrastructure

`Infrastructure` contains concrete implementations of technical and external concerns required by the application.

Responsibilities include:

- Repository implementations.
- Service implementations.
- Persistence implementation.
- Database access.
- Entity Framework configuration where applicable.
- Database migrations.
- Dependency injection extension methods.
- Configuration extension methods.
- Other infrastructure-specific integration code.

`Infrastructure` depends on `Application`.

Dependency direction:

```text
Infrastructure
  -> Application
      -> Domain
          -> Contracts
```

Infrastructure implements abstractions defined by `Application`.

For example:

```text
Application
└── IMemberRepository

Infrastructure
└── MemberRepository : IMemberRepository
```

Concrete infrastructure behavior must not be moved into `Application` merely to avoid creating or using an infrastructure dependency.

## Api

`Api` is the backend presentation and HTTP transport layer of Membriana.

It exposes the application functionality to clients through HTTP endpoints.

Responsibilities include:

- API controllers.
- HTTP request handling.
- HTTP response generation.
- API routing.
- API-specific configuration.
- Backend application startup and composition.
- Dependency injection bootstrap for the backend.
- Mapping HTTP interactions into application operations.

API controllers do not return views.

They return HTTP-oriented responses such as:

- `Ok`.
- `Created`.
- `NoContent`.
- `BadRequest`.
- `NotFound`.
- `Conflict`.
- Other appropriate HTTP responses.

Example:

```csharp
[HttpPost]
public async Task<IActionResult> Create(CreateMemberRequest request)
{
    var result = await _memberService.CreateAsync(request);

    if (!result.Success)
    {
        return Conflict(result);
    }

    return Ok(result);
}
```

`Api` depends on `Infrastructure`.

Dependency direction:

```text
Api
  -> Infrastructure
      -> Application
          -> Domain
              -> Contracts
```

Avoid placing business logic directly in API controllers.

Controllers should primarily:

1. Receive HTTP input.
2. Validate transport-level requirements when appropriate.
3. Delegate work to application or service abstractions.
4. Translate results into HTTP responses.

Do not place Razor views, MVC view models, or browser-facing presentation behavior in `Api`.

## Mvc

`Mvc` is the frontend presentation layer of Membriana.

It is an ASP.NET MVC project responsible for rendering the browser-facing user interface.

Responsibilities include:

- MVC controllers.
- Razor views.
- View models.
- HTTP clients used to communicate with `Api`.
- Presentation-specific transformation logic.
- Browser-facing workflows.
- Frontend configuration.

MVC controllers return views rather than backend HTTP resource responses.

Example:

```csharp
public async Task<IActionResult> Index()
{
    var members = await _memberClient.GetMembersAsync();

    var viewModel = new MembersViewModel
    {
        Members = members
    };

    return View(viewModel);
}
```

`Mvc` communicates with `Api` through HTTP.

Do not bypass the API by directly using backend infrastructure or persistence components from `Mvc`.

The intended communication boundary is:

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

`Mvc` has a direct dependency on `Contracts`.

Dependency direction:

```text
Mvc
  -> Contracts
```

This allows the frontend and backend communication model to share DTOs, enumerations, and other agreed contracts without coupling `Mvc` to backend implementation projects.

`Mvc` must not depend directly on:

- `Infrastructure`.
- `Application`.
- `Domain`.

Frontend-specific models must remain separate from shared transport contracts.

Use view models for data structures whose responsibility is specific to rendering or UI behavior.

For example:

```text
Contracts
└── MemberResponse

Mvc
└── MemberDetailsViewModel
```

Do not move a view model into `Contracts` solely because it contains data received from the API.

## DTOs and View Models

DTOs and view models serve different architectural purposes.

DTOs represent data exchanged across system boundaries.

In Membriana, shared DTOs belong in:

```text
Contracts
```

View models represent data prepared specifically for the frontend presentation.

They belong in:

```text
Mvc
```

Do not treat DTOs and view models as interchangeable concepts.

A view model may:

- Contain multiple DTOs.
- Transform DTO values.
- Add UI-specific state.
- Add display-specific values.
- Combine data from multiple API requests.

## Repository Pattern

Repository abstractions belong in `Application`.

Repository implementations belong in `Infrastructure`.

Example:

```text
Application
└── Repositories
    └── IMemberRepository

Infrastructure
└── Repositories
    └── MemberRepository
```

Do not define a concrete repository inside `Application`.

Do not expose persistence implementation details through repository abstractions unless the architecture explicitly requires them.

## Service Pattern

Service interfaces belong in `Application`.

Concrete service implementations belong in `Infrastructure` under the current Membriana architecture.

Example:

```text
Application
└── Services
    └── IMemberService

Infrastructure
└── Services
    └── MemberService
```

Consumers should depend on abstractions when appropriate instead of directly depending on concrete service implementations.

## Persistence

Persistence is an infrastructure responsibility.

Database access code, Entity Framework configuration, and migrations belong in `Infrastructure`.

The domain entities used by persistence remain defined in `Domain`.

This means the relationship is conceptually:

```text
Domain
    defines entities

Infrastructure
    persists and configures those entities
```

Do not move domain entities into `Infrastructure` merely because Entity Framework maps them to database tables.

Likewise, avoid placing persistence-specific behavior in domain classes unless it is also meaningful as part of the domain model.

## Dependency Injection

Dependency injection configuration that registers infrastructure implementations should be defined in `Infrastructure` when possible.

Extension methods may be used to expose registration entry points to the application host.

Example:

```csharp
services.AddInfrastructure(configuration);
```

The composition root remains responsible for invoking these registrations.

For the backend, this composition normally occurs in `Api`.

Avoid scattering infrastructure registrations across unrelated projects.

## Configuration

Technical configuration helpers may be defined in `Infrastructure` when they configure infrastructure behavior.

Application hosts such as `Api` and `Mvc` remain responsible for loading and composing their own runtime configuration.

Do not move host-specific configuration into lower layers unless it represents reusable configuration owned by that layer.

## Client-Server Boundary

Membriana uses a client-server architecture.

`Mvc` must interact with backend functionality through `Api` using HTTP clients.

The architectural flow is:

```text
Browser
   |
   v
Mvc
   |
   | HTTP
   v
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

Shared contracts may be referenced by both sides where defined by the dependency graph, but application and infrastructure implementations must not cross the HTTP client-server boundary.

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

Correct:

```text
Mvc
  -> HTTP client
      -> Api
```

## Architectural Change Rules

Before introducing a new dependency, project, or architectural responsibility in Membriana:

1. Identify which existing layer owns the responsibility.
2. Prefer using the existing layer if the responsibility already fits it.
3. Verify the current dependency graph.
4. Avoid creating circular dependencies.
5. Avoid bypassing the `Mvc` -> HTTP -> `Api` client-server boundary.
6. Avoid introducing direct dependencies that duplicate an existing dependency path without architectural justification.
7. Keep `Contracts` dependency-free.
8. Keep frontend concerns inside `Mvc`.
9. Keep HTTP backend concerns inside `Api`.
10. Keep infrastructure implementations inside `Infrastructure`.
11. Keep service and repository abstractions inside `Application`.
12. Keep the domain model inside `Domain`.

Do not restructure the architecture merely because another architectural pattern would also be valid.

Architectural consistency within the product takes precedence over personal preference.

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