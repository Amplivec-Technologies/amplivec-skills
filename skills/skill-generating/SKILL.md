---
name: skill-generating
description: Creates, modifies, reviews, and structures Agent Skills for Amplivec projects according to the Agent Skills specification and Amplivec conventions. Use when creating a new skill, modifying an existing skill, reviewing a skill's structure or naming, or defining reusable instructions for AI agents.
---

# Skill Generator

Use this skill whenever creating, modifying, restructuring, or reviewing an Agent Skill used by Amplivec or any Amplivec project.

All generated skills must comply with the official Agent Skills specification published at https://agentskills.io/specification, in addition to the Amplivec-specific conventions defined below.

## Agent Skills specification

Every skill created or modified under Amplivec must comply with the official Agent Skills specification:

https://agentskills.io/specification

Compliance with the specification is mandatory and takes precedence over Amplivec-specific conventions whenever a conflict exists.

At minimum, every skill must comply with the basic requirements defined by the specification, including:

- The skill must be contained in its own directory.
- The directory must contain a `SKILL.md` file.
- `SKILL.md` must begin with valid YAML frontmatter.
- The frontmatter must contain the required `name` and `description` fields.
- `name` must comply with all Agent Skills naming constraints and must match the parent directory name.
- `description` must clearly describe what the skill does and when an agent should use it.
- The content following the frontmatter must be valid Markdown containing the instructions required to perform the skill.
- Optional fields such as `license`, `compatibility`, `metadata`, and `allowed-tools` must follow the constraints defined by the specification when used.
- Optional `scripts/`, `references/`, and `assets/` directories must follow the purpose and conventions established by the specification when present.
- References to files within the skill must use relative paths from the skill root.
- Skills should follow the progressive disclosure principles established by the specification, keeping `SKILL.md` focused and moving detailed supporting material into separate resources when appropriate.

Do not rely exclusively on these summarized requirements. They represent only the baseline expected from every Amplivec skill.

When creating, modifying, restructuring, or reviewing a skill, consult the current official Agent Skills specification whenever access to it is available and ensure that the complete resulting skill complies with it.

Amplivec conventions defined by this skill are additional requirements on top of the Agent Skills specification. They must never be interpreted as replacements for requirements established by the official specification.

## YAML frontmatter

Every `SKILL.md` must begin with valid YAML frontmatter delimited by three hyphens:

```yaml
---
name: skill-name
description: Description of what the skill does and when it should be used.
---
````

At minimum, always include:

* `name`
* `description`

The `name` must comply with the Agent Skills naming restrictions and must match the parent directory name.

The `description` must explain both:

* What the skill does.
* When an agent should use it.

Write descriptions with enough specificity for an agent to reliably determine whether the skill applies to a task.

Do not omit the YAML frontmatter when creating a skill, even if the user only asks for the skill instructions.

When modifying an existing skill, preserve valid existing frontmatter unless the requested changes or these conventions require updating it.

## Skill naming

Amplivec skills follow a semantic naming convention in addition to the technical naming restrictions imposed by Agent Skills.

Skills that represent an action or capability should use a gerund.

Examples:

* `skill-generating`
* `code-reviewing`
* `database-migrating`
* `ui-designing`

The name should describe the capability being practiced rather than the object on which the capability operates.

The exception is a skill that defines standards.

A standards skill should use a name ending in `-standards`.

Examples:

* `architecture-standards`
* `coding-standards`
* `git-standards`

Do not use arbitrary nouns or noun phrases for new skills when the concept can naturally be expressed as a gerund.

Before creating a new skill, determine whether it represents:

1. A capability or activity → use a gerund.
2. A collection of Amplivec standards → use `<domain>-standards`.

When modifying an existing skill whose name violates this convention, identify the inconsistency. Rename it only when the requested scope allows changing the skill name and directory safely.

## Markdown structure

Use normal Markdown headings without numeric prefixes.

Correct:

```markdown
## Naming conventions

## Workflow

## Edge cases
```

Avoid:

```markdown
## 1. Naming conventions

## 2. Workflow

## 3. Edge cases
```

Do not manually number sections or subsections using heading text.

Lists and workflow steps may still be numbered when sequence matters.

## Visual separators

Do not use horizontal rules as visual separators between sections.

Avoid standalone Markdown horizontal rules such as:

```markdown
---
```

or equivalent separators inside the Markdown body.

The YAML frontmatter delimiters at the beginning of `SKILL.md` are required and are the exception to this rule.

Prefer heading hierarchy and whitespace to visually separate sections.

## Instruction design

Write skills as operational instructions for an AI agent rather than as documentation intended primarily for humans.

Instructions should be explicit enough to produce repeatable behavior.

Prefer directives such as:

* Always...
* Never...
* When...
* If...
* Before...
* After...
* Prefer...
* Only when...

Avoid unnecessary explanatory prose when a concise rule communicates the same requirement.

Explain the reason behind a rule when the reason helps the agent make decisions in cases not explicitly covered by the skill.

## Scope

A skill should have a clear and coherent responsibility.

Do not turn a skill into a collection of unrelated instructions.

Before adding instructions to an existing skill, determine whether they belong to that skill's responsibility. If they describe a substantially different capability, recommend creating another skill instead.

Avoid duplicating rules already defined by another Amplivec skill when they can be referenced or composed instead.

## Progressive disclosure

Keep the main `SKILL.md` focused on instructions the agent commonly needs when the skill is activated.

Do not place large amounts of reference information in `SKILL.md` when that information is only needed for specific situations.

When appropriate, move supporting material into:

* `references/` for detailed documentation or specifications.
* `scripts/` for executable helpers.
* `assets/` for templates and static resources.

Reference those files from `SKILL.md` using relative paths.

Do not create supporting files or directories unless they provide actual value to the skill.

## Creating a skill

When asked to create a new skill:

1. Determine the exact capability the skill represents.
2. Determine whether it is an activity skill or a standards skill.
3. Choose a compliant Amplivec skill name.
4. Verify that the name also satisfies the Agent Skills specification.
5. Create the YAML frontmatter with at least `name` and `description`.
6. Write the instructions as operational directives for the agent.
7. Organize the Markdown using unnumbered headings.
8. Do not add horizontal-rule separators.
9. Determine whether references, scripts, or assets are necessary.
10. Review the complete skill for ambiguity, duplication, unnecessary verbosity, and conflicts with other instructions.
11. Verify the final structure against the current Agent Skills specification.

## Modifying a skill

When asked to modify an existing skill:

1. Read the complete existing `SKILL.md` before making structural decisions.
2. Preserve instructions unrelated to the requested modification.
3. Check whether the existing skill complies with the current Agent Skills specification.
4. Check whether it complies with Amplivec naming and formatting conventions.
5. Integrate the requested behavior into the most appropriate existing section instead of blindly appending new sections.
6. Remove or rewrite rules that would directly contradict the requested behavior.
7. Avoid duplicating equivalent instructions.
8. Preserve useful references, scripts, assets, and metadata.
9. Verify the resulting skill as a complete artifact rather than validating only the changed fragment.

Do not perform unrelated semantic changes merely because other improvements are possible.

## Final review

Before considering a created or modified skill complete, verify:

* `SKILL.md` exists.
* YAML frontmatter is present and valid.
* `name` is present.
* `description` is present.
* The directory name and `name` match.
* The name satisfies the current Agent Skills specification.
* The name follows the Amplivec gerund or `-standards` convention.
* The description explains what the skill does and when to use it.
* Headings are not manually numbered.
* The Markdown body does not contain decorative horizontal rules.
* Instructions are operational and unambiguous.
* The skill has a coherent responsibility.
* Supporting resources are included only when useful.
* Existing behavior unrelated to a modification has not been accidentally removed.
* The complete result complies with the current official Agent Skills specification.
