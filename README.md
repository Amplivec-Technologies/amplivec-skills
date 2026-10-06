# Amplivec Skills

Shared engineering skills, standards and AI agent resources for the Amplivec ecosystem.

## Purpose

Amplivec projects are developed independently, but they belong to the same engineering ecosystem.

Instead of duplicating common instructions and practices across repositories, this repository provides a central place for reusable resources such as:

- AI agent skills
- Engineering standards and conventions
- Development workflows
- Documentation guidelines
- Testing practices
- Architecture guidelines
- Git and repository conventions
- Reusable templates and tooling

Project-specific knowledge should remain in the corresponding product repository.

## Setup

Add this repository as a Git submodule from the root of the project that will consume the shared resources:

```bash
git submodule add https://github.com/mrmalvicino/amplivec-skills.git amplivec-skills
```

The `amplivec-skills` argument is the destination directory within the consuming project.
Adjust it to match the project's structure and use that path in subsequent commands.
Shared skills are available under `amplivec-skills/skills/`.

If you use OpenCode, use `.opencode` as the submodule destination instead:

```bash
git submodule add https://github.com/mrmalvicino/amplivec-skills .opencode
```

This makes the shared skills available under `.opencode/skills/` for OpenCode to discover. Use `.opencode` in place of `amplivec-skills` in the update commands below.

When cloning a project that already includes the submodule, initialize it along with the project:

```bash
git clone --recurse-submodules <project-repository-url>
```

For an existing clone, run this from the project root. Repeat it after pulling changes or switching branches to ensure the local submodule matches the reference recorded by the project, rather than continuing to use an older checkout of Amplivec Skills:

```bash
git submodule update --init --recursive
```

## Guiding principles

Resources maintained here should follow a few basic principles:

**Reusable**  
They should provide value to more than one Amplivec project whenever possible.

**Focused**  
Each skill, standard or guideline should address a clearly defined responsibility.

**Technology-aware, not technology-bound**  
Technology-specific resources are allowed, but general engineering conventions should avoid unnecessary coupling to a particular framework or language.

**Explicit**  
Engineering decisions and conventions should be documented rather than depending exclusively on implicit team knowledge.

**Evolvable**  
Standards are expected to change as Amplivec and its products mature.

## Contributing

To add or modify skills, work in a separate clone of the [source repository](https://github.com/mrmalvicino/amplivec-skills), not in the submodule directory of a consuming project.
Submit changes to the source repository first.
Once merged, update the submodule reference in each consuming project as described in Setup.

When adding a new resource:

1. Determine whether the knowledge is shared across Amplivec or specific to a single product.
2. Place product-specific knowledge in the corresponding product repository.
3. Keep shared resources focused on a single responsibility.
4. Prefer extending an existing resource instead of introducing overlapping instructions.
5. Document relevant assumptions and constraints.

Changes to shared engineering standards should be reviewed considering their impact on all projects that may consume them.

## About us

Amplivec is an initiative focused on building software products through a collaborative engineering ecosystem, combining independent projects with shared technical standards, knowledge and identity. Visit [Amplivec Technologies website](https://amplivec.com) for more info.
