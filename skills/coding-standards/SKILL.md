---
name: coding-standards
description: Apply Amplivec coding style and naming conventions when creating, modifying, refactoring, or reviewing source code.
---

# Amplivec Coding Standards

Apply these conventions whenever creating, modifying, refactoring, or reviewing source code in an Amplivec project.

When editing existing code, apply these rules to the code being changed. Do not perform unrelated formatting changes unless explicitly requested.

## Naming Conventions

Unless the user or project explicitly specifies a different naming convention, prioritize the standard, most widely recognized naming conventions of the language and its ecosystem.

Apply this principle to functions, methods, variables, parameters, fields, properties, constants, types, modules, and files. Do not impose one language's naming conventions on another language. In particular, the C# conventions below are not universal requirements for JavaScript, Python, or other languages.

When several naming styles are conventional in a language, follow the established convention of the project or framework. For languages not listed here, use their official style guide or the most widely adopted community convention.

### C#

Use the following conventions for C# unless explicitly overridden:

- Local variables must use `camelCase`.
- Private fields must use `camelCase` prefixed with an underscore (`_`).
- Public properties must use `PascalCase`.
- Classes must use `PascalCase`.
- Methods must use `PascalCase`.
- Class file names must use `PascalCase`.

Examples:

```csharp
private string _customerName;
private decimal _totalAmount;

public string CustomerName { get; set; }
public decimal TotalAmount { get; set; }

public void CalculateTotal()
{
    decimal subtotal = GetSubtotal();
}
```

When creating a C# class, its file name should match the class name whenever possible.

### JavaScript and TypeScript

Use the following conventions unless explicitly overridden:

- Variables, parameters, functions, methods, fields, and ordinary object properties use `camelCase`.
- Classes and constructor functions use `PascalCase`. TypeScript types and interfaces also use `PascalCase`.
- Symbolic constants may use `UPPER_SNAKE_CASE` when appropriate for the project. Do not uppercase every identifier merely because it is declared with `const`.
- Follow the project's private-member convention, such as native `#privateField` syntax. Do not introduce an underscore prefix solely because C# uses it.
- Follow the project's or framework's established file-naming convention rather than automatically assigning `PascalCase` to every JavaScript or TypeScript file.

Examples: `calculateQuote`, `rentCents`, `quote.premiumCents`, `QuoteCalculator`, and `PAYMENT_DISCOUNT_PERCENT`.

### Python

Use the following conventions unless explicitly overridden:

- Variables, parameters, functions, methods, attributes, and properties use `snake_case`.
- Classes use `PascalCase` (the `CapWords` convention in PEP 8).
- Constants use `UPPER_SNAKE_CASE`.
- Non-public members conventionally use a leading underscore, as in `_total_amount`.
- Modules use short lowercase names, with underscores where they improve readability. Packages conventionally use short lowercase names without underscores.

Examples: `calculate_quote`, `rent_cents`, `quote.premium_cents`, `QuoteCalculator`, and `PAYMENT_DISCOUNT_PERCENT`.

### Existing Names and External Contracts

Preserve names required by language syntax, standard libraries, frameworks, third-party APIs, serialization formats, and other external contracts. Do not rename them merely to normalize casing. Limit other naming changes to the requested scope and update all affected references consistently.

These language-specific naming defaults do not override the separate formatting and whitespace rules in this skill.

## Language

All source code identifiers, project files, project structure, and database identifiers must be written in English.

This includes:

- File names
- Directory names
- Classes
- Methods
- Properties
- Fields
- Variables
- Constants
- Database tables
- Database columns
- Other database identifiers

Do not introduce Spanish identifiers into source code or database schemas.

Code documentation, however, must be written in Spanish.

This includes:

- Source code comments
- XML documentation comments
- `<summary>` documentation for classes and methods
- `<remarks>` documentation for classes and methods
- Other documentation embedded directly in source code

The language of the code and the language of its documentation must therefore remain separate: identifiers must be written in English, while documentation explaining the code must be written in Spanish.

## Comments and Code Documentation

All comments and code documentation must be written in Spanish.

### Single-Line Comments

Comments referring specifically to a single line of code should be inline and placed to the right of that line.

Example:

```csharp
var total = CalculateTotal(); // Calcula el importe total de la operación.
```

### Block Comments

Comments referring to a block of code must be placed above the block.

Leave one blank line between the comment and the code it describes.

Example:

```csharp
// Valida que todos los datos necesarios estén disponibles.

if (customer != null)
{
    ProcessCustomer(customer);
}
```

Avoid comments that merely restate what the code already expresses clearly.

### XML Documentation

XML documentation for classes and methods must be written in Spanish.

This includes both `<summary>` and `<remarks>` sections.

Example:

```csharp
/// <summary>
/// Procesa la información correspondiente al cliente.
/// </summary>
/// <remarks>
/// La operación solo se realiza cuando el cliente contiene la información
/// necesaria para completar el procesamiento.
/// </remarks>
public void ProcessCustomer(Customer customer)
{
    // ...
}
```

Code identifiers referenced from documentation must preserve their original English names.

## Indentation

Use tabs for indentation.

Each tab must represent an indentation width equivalent to 4 spaces.

Do not replace indentation tabs with spaces.

Continuation indentation should remain consistent with the surrounding code and preserve the equivalent 4-space indentation level.

## Spaces

Leave a space after commas.

```csharp
Method(firstArgument, secondArgument);
```

Do not add a space between a method name and its opening parenthesis.

Correct:

```csharp
CalculateTotal();
```

Incorrect:

```csharp
CalculateTotal ();
```

Use a space before parentheses belonging to control flow structures.

Correct:

```csharp
if (condition)
```

Incorrect:

```csharp
if(condition)
```

## Control Flow Structures

Single-line control flow structures are prohibited.

In languages that use braces to delimit blocks, all control flow structures must use braces, even when their body contains only one statement. In languages such as Python, use the native block syntax with the body on subsequent indented lines.

This applies to structures such as:

- `if`
- `else`
- `for`
- `foreach`
- `while`
- `do`
- Other equivalent control flow structures

Place opening and closing braces according to the language-specific defaults in the **Braces** section. Do not apply C# brace placement universally.

In brace-based languages, the body of the structure must begin on the line following the opening brace. An opening brace on the same line as the header does not make the structure a prohibited single-line structure; placing the body on that same line does.

Correct for C# using the default Allman style:

```csharp
if (condition)
{
    Execute();
}
```

Incorrect:

```csharp
if (condition) Execute();
```

Incorrect:

```csharp
if (condition)
    Execute();
```

Incorrect for C# using the default Allman style:

```csharp
if (condition) {
    Execute();
}
```

The same requirement to use multiline bodies applies to other control flow structures. The following examples use C# brace placement; use the corresponding language's style in other languages.

Correct:

```csharp
for (var i = 0; i < items.Count; i++)
{
    Process(items[i]);
}
```

Correct:

```csharp
while (condition)
{
    Execute();
}
```

Correct:

```csharp
foreach (var item in items)
{
    Process(item);
}
```

In brace-based languages, never omit braces because a control flow structure currently contains only one statement.

### Prefer Explicit Control Flow

Prioritize simple and explicit control flow over concise but dense expressions.

It is acceptable to use more lines when they make each decision and possible outcome easier to understand.

Do not combine several responsibilities in a single condition when separate branches would be clearer. This includes combinations of:

- Null checking.
- Type checking or pattern matching.
- Variable declaration.
- Business-rule validation.
- Selection of the value to return.

Prefer one clear decision per `if`, early returns, and explicit branches for a small set of known alternatives.

Do not introduce an interface cast or pattern-matching expression solely to make the control flow shorter when the concrete cases are already known.

Incorrect:

```csharp
Person? person = user.Employee ?? (Person?)user.Member;

if (person is ITenantable tenantable &&
    tenantable.OrganizationId != user.OrganizationId)
{
    throw new KeyNotFoundException();
}

return person;
```

Correct:

```csharp
if (user.Employee != null)
{
    if (user.Employee.OrganizationId != user.OrganizationId)
    {
        throw new KeyNotFoundException();
    }

    return user.Employee;
}

if (user.Member != null)
{
    if (user.Member.OrganizationId != user.OrganizationId)
    {
        throw new KeyNotFoundException();
    }

    return user.Member;
}

return null;
```

Compact constructs remain appropriate when they express a single, immediately understandable decision. The goal is not to prohibit pattern matching or compound conditions, but to avoid using them when they obscure the actual branches of the behavior.

## Blank Lines

Do not leave blank lines immediately after an opening brace.

Do not leave blank lines immediately before a closing brace.

Correct:

```csharp
if (condition)
{
    Execute();
}
```

Incorrect:

```csharp
if (condition)
{

    Execute();

}
```

### Blank Lines Around Control Flow Structures

Control flow structures must be visually separated from adjacent statements within the same scope.

Leave exactly one blank line before a control flow structure when another statement precedes it within the same scope.

Leave exactly one blank line after a control flow structure when another statement follows it within the same scope.

Example:

```csharp
var customer = GetCustomer();

if (customer != null)
{
    ProcessCustomer(customer);
}

SaveChanges();
```

When two control flow structures are consecutive within the same scope, leave exactly one blank line between them.

Correct:

```csharp
if (firstCondition)
{
    ExecuteFirstAction();
}

if (secondCondition)
{
    ExecuteSecondAction();
}
```

Do not add a blank line before a control flow structure when it is the first statement in its scope.

Correct:

```csharp
public void Process()
{
    if (condition)
    {
        Execute();
    }

    SaveChanges();
}
```

Do not add a blank line after a control flow structure when it is the last statement in its scope.

Correct:

```csharp
public void Process()
{
    Prepare();

    if (condition)
    {
        Execute();
    }
}
```

When a control flow structure is the only statement in a scope, do not add blank lines before or after it.

Correct:

```csharp
public void Process()
{
    if (condition)
    {
        Execute();
    }
}
```

Correct:

```csharp
public void ProcessItems()
{
    foreach (var item in items)
    {
        Process(item);
    }
}
```

Do not introduce blank lines solely because a control flow structure begins or ends a scope.

### Blank Lines Around Multiline Statements

Apply this rule in every programming language, including C#, Python, and JavaScript.

Declarations, definitions, assignments, and expression statements that span multiple lines must be visually separated from adjacent statements in the same enclosing scope. This includes collection and object initializers, chained calls, and statements containing multiline lambdas, arrow functions, or anonymous functions, whether split for line length or readability.

- Leave exactly one blank line before the complete statement when another statement precedes it in the same scope.
- Leave exactly one blank line after the complete statement when another statement follows it in the same scope.
- Do not add a blank line before it when it is the first statement in its scope.
- Do not add a blank line after it when it is the last statement in its scope.
- If it is the only statement in its scope, do not add blank lines on either side.
- Between consecutive multiline statements or a multiline statement and a control flow structure, use exactly one blank line, not two.

Treat the entire declaration, assignment, or expression as one logical statement, including its closing delimiters and terminator. Do not insert blank lines between continuation lines, initializer elements, arguments, or successive closing delimiters solely because the statement is multiline. Apply the same rules independently to statements inside a lambda or anonymous function body.

Example:

```javascript
const title = "Resumen";

const rows = [
	["Alquiler mensual", rent],
	["Expensas mensuales", expenses],
];

const formatRow = row =>
	row.join(": ");

renderRows(title, rows, formatRow);
```

### Blank Lines After Closing Braces

Leave a blank line after a closing brace when another independent statement or structure follows within the same scope.

Do not leave a blank line between successive closing braces.

Do not insert a blank line between a closing brace and an associated `else`, `catch`, or `finally` block.

Correct:

```csharp
if (condition)
{
    Execute();
}
else
{
    ExecuteAlternative();
}
```

Correct:

```csharp
if (condition)
{
    while (otherCondition)
    {
        Execute();
    }
}
```

## Braces

Unless the user or project explicitly requires a different brace style, prioritize the standard, most widely recognized style of the language and its ecosystem.

Use these defaults:

- **C#:** use Allman style, with opening and closing block braces on separate lines aligned with the declaration or control-flow header.
- **JavaScript and TypeScript:** use 1TBS (One True Brace Style, also called Egyptian braces), with the opening block brace at the end of the header's final line.
- **Other languages with an established convention:** follow their official style guide or most widely adopted community convention. When several styles are equally conventional, follow the project's or framework's established style.
- **Brace-based languages without an established convention:** use Allman as the fallback, aligning opening and closing block braces on separate lines.
- **Languages without block braces, such as Python:** use their native block syntax. Do not introduce braces to imitate another language.

Apply the selected block style consistently to functions, methods, constructors, classes, control flow structures, and block-bodied lambdas or anonymous functions. The separate rules for indentation, multiline bodies, and blank lines still apply.

### Allman: C# and Fallback

Place opening and closing block braces on their own lines, aligned with the corresponding declaration or control-flow header. Start the body on the next line and indent it one level.

Correct for C#:

```csharp
public void Execute()
{
    Process();
}
```

Incorrect for C# using the default Allman style:

```csharp
public void Execute() {
    Process();
}
```

### 1TBS: JavaScript and TypeScript

Place the opening block brace on the same line as the declaration or control-flow header, separated by a space. If the header spans multiple lines, put the brace at the end of the final header line. Start the body on the next line and indent it one level.

Place the closing brace on its own line, aligned with the start of the declaration or control-flow header. Associated clauses follow that closing brace on the same line: `} else {`, `} else if (...) {`, `} catch (...) {`, `} finally {`, and `} while (...);` for `do`/`while`. Required punctuation may follow a closing brace, as in `};` or `});`.

Correct for JavaScript:

```javascript
function processCustomer(customer) {
	if (customer !== null) {
		saveCustomer(customer);
	} else {
		showMissingCustomer();
	}
}
```

Correct for a JavaScript callback:

```javascript
customers.forEach((customer) => {
	processCustomer(customer);
});
```

Do not force Allman placement on JavaScript or TypeScript unless it is explicitly required. Using 1TBS does not permit single-line control flow bodies.

### Syntax and Non-Block Braces

Brace-placement rules for blocks do not require every brace in source code to occupy its own line. For object literals, destructuring, named imports, and initializer expressions, follow the language's syntax and idiomatic formatting.

Preserve semantics when wrapping expressions. In JavaScript, do not place a line break immediately after `return` before its expression; automatic semicolon insertion can change the behavior. Keep the expression on that line or begin a parenthesized multiline expression on that line.

## Line Length

Source code lines must not exceed the maximum line length defined by the Amplivec coding standards.

When a condition, method invocation, expression, declaration, or other statement would exceed that limit, split it across multiple lines while preserving readability and indentation.

Apply the blank-line rules for multiline statements to the resulting statement.

Do not keep an excessively long condition on a single line.

## HTML Formatting

When creating, modifying, refactoring, or reviewing HTML documents or partials, read and apply [HTML formatting conventions](references/html-formatting.md).

- Use two spaces per markup nesting level and per attribute or text continuation level. This is an HTML-specific exception to the general tab-indentation rule; it does not change indentation rules for other languages.
- Expand structural containers and separate independent controls, fields, navigation links, and repeated items into readable source lines. Make parent-child relationships visible through indentation.
- Prefer separate opening and closing lines for expanded containers, with closing tags aligned to their opening element's indentation level.
- Keep short text-only elements, coherent inline fragments, and empty paired elements compact when appropriate. Do not interpret the standard as an absolute prohibition on multiple tags appearing on one line.
- Wrap long opening tags between complete attributes, indent continuation lines by two spaces, and keep `>` with the final attribute. Do not require one attribute per line or invent a numeric line-length limit.
- Use blank lines to separate major sections or logical groups, not every HTML element. Do not apply the blank-line rules for programming-language statements mechanically to markup.
- Preserve rendered text, meaningful inline whitespace, attribute values, element order, and behavior when formatting. Formatting alone does not authorize changing copy, removing fallbacks, or modifying functionality.

## Existing Code

When modifying existing code:

1. Follow these standards for all newly introduced code.
2. Apply the standards to nearby code when necessary to keep the modified section consistent.
3. Do not reformat unrelated files or unrelated sections solely to enforce these conventions.
4. Preserve existing behavior unless the task explicitly requires behavioral changes.

## Code Review

When reviewing code, identify violations of these conventions when they affect code introduced or modified by the change.

Distinguish style issues from functional defects.

Do not describe a style violation as a functional bug.
