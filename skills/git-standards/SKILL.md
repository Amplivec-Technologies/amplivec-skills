---
name: git-standards
description: Apply Amplivec Git conventions when creating commits, writing commit messages, creating or managing branches, merging changes, or defining a repository branching strategy.
---

# Amplivec Git Standards

Apply these conventions whenever creating commits, writing commit messages, creating or managing branches, merging changes, or working with the Git history of an Amplivec repository.

The objective is to maintain a clear, traceable, and readable Git history while keeping commits small and logically cohesive.

## Commit Messages

Commit messages must follow a simplified Conventional Commits structure adapted to Amplivec projects.

The commit subject must use the following format:

```text
<type>: <short description in Spanish> [<jira-tag>]
```

Example:

```text
feat: agrega búsqueda de clientes por CUIT [MEM-142]
```

The commit type must:

- Be written in lowercase.
- Appear at the beginning of the commit subject.
- Be immediately followed by a colon and a space.

The commit description must:

- Always be written in Spanish.
- Be written in the simple present tense.
- Describe what the commit does.
- Briefly describe the primary change introduced by the commit.
- Appear after the commit type and before the Jira tag.

The Jira issue tag must:

- Appear at the end of the commit subject.
- Be enclosed in square brackets.
- Match the Jira issue associated with the change.

Example:

```text
fix: evita la generación de pagos duplicados [MEM-218]
```

## Commit Language

All human-readable commit message content must be written in Spanish.

This requirement applies to:

- The short description in the commit subject.
- The detailed commit body.
- Every item included in the detailed commit body.
- Any additional explanatory text included in the commit message.

Technical identifiers must preserve their original names and must not be translated.

This includes:

- Class names.
- Method names.
- Property names.
- Variable names.
- File names.
- Database identifiers.
- Branch names.
- Library or framework names.
- Other technical identifiers.

Correct:

```text
refactor: extrae la lógica de estados a MemberStatusService [MEM-417]

- Mueve la validación de estados desde MemberController hacia MemberStatusService.
- Actualiza la inyección de IMemberStatusService.
- Adapta ProcessMember para utilizar el nuevo servicio.
```

Incorrect:

```text
refactor: extract status logic to MemberStatusService [MEM-417]

- Move status validation from MemberController to MemberStatusService.
- Update dependency injection.
```

Do not write commit descriptions or detailed commit bodies in English.

## Commit Tense

Both the commit subject description and every item in the detailed commit body must be written in the simple present tense.

Commit messages must describe what the commit does, not what should be done, what was done, or the action in infinitive form.

Use conjugated verbs in the simple present tense.

Correct:

```text
feat: agrega filtro para buscar clientes [MEM-142]
```

Incorrect:

```text
feat: agregar filtro para buscar clientes [MEM-142]
```

Correct:

```text
fix: corrige la validación del cliente seleccionado [MEM-218]
```

Incorrect:

```text
fix: corregir la validación del cliente seleccionado [MEM-218]
```

Incorrect:

```text
fix: se corrigió la validación del cliente seleccionado [MEM-218]
```

The same rule applies to the detailed commit body.

Correct:

```text
refactor: reorganiza el manejo de estados de membresía [MEM-417]

- Extrae la lógica de transición de estados a MemberStatusService.
- Mueve la validación de estados fuera de MemberController.
- Actualiza los registros de inyección de dependencias.
- Adapta los consumidores existentes para utilizar el nuevo servicio.
```

Incorrect:

```text
refactor: reorganizar el manejo de estados de membresía [MEM-417]

- Extraer la lógica de transición de estados a MemberStatusService.
- Mover la validación de estados fuera de MemberController.
- Se actualizaron los registros de inyección de dependencias.
- Adaptación de los consumidores existentes para utilizar el nuevo servicio.
```

Prefer direct verbs that clearly describe the effect of the commit, such as:

- `agrega`
- `actualiza`
- `corrige`
- `elimina`
- `extrae`
- `incorpora`
- `implementa`
- `mueve`
- `reemplaza`
- `refactoriza`
- `renombra`
- `reorganiza`
- `simplifica`
- `unifica`

## Commit Types

Use the following commit types when applicable:

- `feat`: Introduces a new feature or user-facing capability.
- `fix`: Fixes a defect or incorrect behavior.
- `refactor`: Changes the internal structure of the code without intentionally changing its external behavior.
- `docs`: Changes documentation without modifying application behavior.
- `test`: Adds, updates, or fixes tests.
- `chore`: Performs maintenance work that does not directly modify application functionality.
- `perf`: Improves application performance without changing its intended behavior.
- `style`: Changes formatting, whitespace, naming, or other code style aspects without changing behavior.

Choose the type that best represents the primary purpose of the commit.

Do not combine unrelated types of changes in the same commit solely to avoid creating multiple commits.

## Commit Subject

The commit subject must provide a brief description in Spanish of the primary change introduced by the commit.

The description must be written in the simple present tense and state what the commit does.

Keep the subject concise and focused on what the commit accomplishes.

Correct:

```text
feat: agrega validación de vencimiento de membresías [MEM-305]
```

Correct:

```text
refactor: extrae el procesamiento de pagos a un servicio [MEM-322]
```

Avoid infinitive verbs.

Incorrect:

```text
feat: agregar validación de vencimiento de membresías [MEM-305]
```

Avoid past-tense descriptions.

Incorrect:

```text
feat: se agregó validación de vencimiento de membresías [MEM-305]
```

Avoid vague descriptions.

Incorrect:

```text
fix: cambios [MEM-305]
```

Incorrect:

```text
chore: cambios varios [MEM-305]
```

The detailed commit body should be used when the subject alone is not sufficient to explain the changes.

## Commit Body

Small and self-explanatory commits do not require a detailed body.

For medium or large commits, a detailed description may be added below the commit subject.

The entire commit body must be written in Spanish, except for technical identifiers that must preserve their original names.

Every item must be written in the simple present tense and describe what the commit does.

Separate the subject from the body with a blank line.

Use hyphen-prefixed items to describe the individual changes introduced by the commit.

Example:

```text
refactor: reorganiza el manejo de estados de membresía [MEM-417]

- Extrae la lógica de transición de estados a MemberStatusService.
- Mueve la validación de estados fuera de MemberController.
- Actualiza los registros de inyección de dependencias.
- Adapta los consumidores existentes para utilizar el nuevo servicio.
```

There is no maximum line length for detailed commit body items.

Each item may use as many characters as necessary to clearly describe the corresponding change.

Do not split an item into multiple lines solely to enforce a character limit.

The body should describe relevant implementation changes without unnecessarily repeating information already evident from the subject.

## Commit Size

Commits should be small, focused, and logically cohesive.

Prefer multiple small commits over a single large commit when the changes can be separated into meaningful units.

Each commit should ideally represent one logical change.

Avoid combining:

- Unrelated features.
- Independent bug fixes.
- Refactors unrelated to the primary change.
- Formatting changes unrelated to the implementation.
- Multiple independent responsibilities.

Large commits are discouraged because they make code review, history inspection, debugging, reverting, and cherry-picking more difficult.

A larger commit is acceptable when the changes form a single logical unit that cannot reasonably be divided without leaving the repository in an inconsistent or invalid state.

Do not split a logically atomic change merely to reduce the number of modified files.

## Jira Traceability

Commits associated with Jira work must include the corresponding Jira issue tag.

Format:

```text
[JIRA-TAG]
```

Example:

```text
feat: implementa el flujo de invitaciones a organizaciones [MEM-521]
```

All commits belonging to the same Jira issue may use the same Jira tag.

A Jira issue does not need to correspond to a single commit. Prefer creating multiple focused commits when the issue requires several logically independent changes.

## Gitmoji

Gitmoji may optionally be used for commits that are especially significant or where the emoji provides useful visual context in the Git history.

Follow the conventions defined by `gitmoji.dev` when selecting the emoji.

Gitmoji is optional and should not be added mechanically to every commit.

When used, the Gitmoji must appear at the beginning of the commit subject, before the commit type.

Format:

```text
<gitmoji> <type>: <short description in Spanish> [<jira-tag>]
```

Example:

```text
✨ feat: agrega el flujo de invitaciones a organizaciones [MEM-521]
```

Example:

```text
♻️ refactor: reorganiza el manejo de estados de membresía [MEM-417]
```

The Conventional Commit type remains mandatory even when a Gitmoji is present.

## Branching Strategy

Amplivec repositories use a simplified Git Flow branching strategy when the project requires multiple permanent environments or coordinated development between multiple developers.

The required branching strategy depends on the maturity of the project and the number of developers working on it.

## Simple Repository Workflow

A repository may work directly on `main` when at least one of the following conditions applies:

- The project is not yet in production.
- The repository is maintained by a single developer.

In these cases, using `develop`, `testing`, and temporary feature branches is optional.

Direct commits to `main` are permitted.

This simplified workflow avoids unnecessary branching overhead for early-stage or individually maintained projects.

## Git Flow Workflow

The Git Flow workflow becomes mandatory when at least one of the following conditions applies:

- The project is in production.
- More than one developer actively works on the repository.

Under this workflow, the following permanent branches must exist:

```text
main
develop
testing
```

These branches are permanent and must not be deleted.

Their responsibilities are:

### `main`

Represents production-ready code.

Changes reaching `main` must have already passed through the development and testing stages.

`main` must always represent the production state or code ready to be deployed to production.

### `develop`

Represents the integration branch for active development.

New feature branches must originate from `develop`.

Completed feature work is merged back into `develop` before progressing to testing.

### `testing`

Represents code that has passed development integration and is being validated before production.

Changes move from `develop` to `testing` after they are considered ready for validation.

Once validated, they progress from `testing` to `main`.

## Merge Flow

Changes must always progress through the permanent branches in the following direction:

```text
feat/<feature-name>
        |
        v
     develop
        |
        v
     testing
        |
        v
      main
   (production)
```

The required merge flow is therefore:

1. `feat/<feature-name>` → `develop`
2. `develop` → `testing`
3. `testing` → `main`

This order must be preserved.

Feature branches must not be merged directly into `testing` or `main`.

Changes from `develop` must not bypass `testing` and be merged directly into `main`.

`main` represents the final production stage of the merge flow.

## Feature Branches

All temporary branches used to develop features must be created from `develop`.

Feature branches must use the following naming convention:

```text
feat/<feature-name>
```

Examples:

```text
feat/member-import
feat/organization-invitations
feat/payment-history
```

Feature branch names must:

- Use the `feat/` prefix.
- Be written in English.
- Use lowercase letters.
- Use hyphens to separate words.
- Clearly identify the feature being developed.
- Avoid unnecessarily long names.

When useful for traceability, the Jira issue tag may be included in the branch name.

Example:

```text
feat/mem-521-organization-invitations
```

Do not use alternative prefixes such as `feature/` for feature development branches.

## Feature Branch Merge Strategy

When a feature branch is completed, it must be integrated into `develop` using a squash merge.

The commits created during development of the feature branch are therefore consolidated into a single commit when the feature is merged into `develop`.

Example:

```text
feat/member-import
        |
        | squash merge
        v
     develop
```

The resulting squashed commit in `develop` must follow the commit message conventions defined by this skill, including the requirements that:

- The description is written in Spanish.
- The detailed body is written in Spanish.
- The description uses the simple present tense.
- Every detailed body item uses the simple present tense.
- Technical identifiers preserve their original names.

The purpose of squash merging feature branches is to:

- Keep the permanent branch history concise.
- Prevent intermediate or corrective development commits from unnecessarily polluting the shared history.
- Preserve each completed feature as a clear logical unit in `develop`.
- Make the history easier to review, revert, and understand.

Individual commits inside a feature branch should still be small, focused, and meaningful while development is in progress.

Squash merging does not justify creating unnecessarily large or poorly structured commits during development.

The squash requirement specifically applies when merging a `feat/` branch into `develop`.

## Feature Branch Lifecycle

Feature branches are temporary.

The expected lifecycle is:

1. Create a `feat/<feature-name>` branch from `develop`.
2. Implement the required changes.
3. Keep development commits small and logically focused.
4. Complete and review the feature.
5. Squash merge the feature branch into `develop`.
6. Promote the integrated changes from `develop` to `testing`.
7. Validate the changes in `testing`.
8. Merge the validated changes from `testing` to `main`.
9. Deploy or otherwise make the `main` version available in production.
10. Once the feature has successfully reached production, the temporary feature branch may be deleted.

The complete lifecycle is:

```text
develop
   |
   | create branch
   v
feat/<feature-name>
   |
   | development commits
   |
   | squash merge
   v
develop
   |
   | merge
   v
testing
   |
   | validation
   |
   | merge
   v
main
   |
   v
production
```

The following permanent branches must never be deleted as part of the normal feature lifecycle:

```text
main
develop
testing
```

Temporary `feat/` branches may be deleted after their changes have successfully reached production.

## General Git Principles

When working with Git:

- Keep commits small and logically cohesive.
- Prefer several meaningful commits over one large commit.
- Do not mix unrelated changes in the same commit.
- Write all commit descriptions in Spanish.
- Write all detailed commit body items in Spanish.
- Write commit descriptions in the simple present tense.
- Write every detailed commit body item in the simple present tense.
- Describe what the commit does rather than using infinitive or past-tense phrasing.
- Preserve technical identifiers in their original language and form.
- Write commit subjects that clearly explain the purpose of the change.
- Include Jira traceability when the work belongs to a Jira issue.
- Do not create unnecessary branches for simple, single-developer, or pre-production repositories.
- Use the simplified Git Flow workflow once the project is in production or has multiple active developers.
- Treat `main`, `develop`, and `testing` as permanent branches when Git Flow is active.
- Create feature branches from `develop`.
- Name feature branches using the `feat/<feature-name>` convention.
- Squash merge completed feature branches into `develop`.
- Promote changes strictly through `feat/<feature-name>` → `develop` → `testing` → `main`.
- Never bypass a stage of the established merge flow.
- Treat `main` as the production branch.
- Delete temporary feature branches only after their changes have successfully reached production.
- Preserve a Git history that makes it easy to understand why and when a change was introduced.