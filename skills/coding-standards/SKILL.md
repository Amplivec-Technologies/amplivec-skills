---
name: coding-standards
description: Apply Amplivec coding style and naming conventions when creating, modifying, refactoring, or reviewing source code.
---

# Amplivec Coding Standards

Apply these conventions whenever creating, modifying, refactoring, or reviewing source code in an Amplivec project.

When editing existing code, apply these rules to the code being changed. Do not perform unrelated formatting changes unless explicitly requested.

## Naming Conventions

Use the following naming conventions:

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

When creating a class, its file name should match the class name whenever possible.

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

All control flow structures must use braces, even when their body contains only one statement.

This applies to structures such as:

- `if`
- `else`
- `for`
- `foreach`
- `while`
- `do`
- Other equivalent control flow structures

The opening brace must be placed on a separate line.

The body of the structure must begin on the line following the opening brace.

Correct:

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

Incorrect:

```csharp
if (condition) {
    Execute();
}
```

The same rule applies to other control flow structures.

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

Never omit braces because a control flow structure currently contains only one statement.

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

RenderRows(title, rows, formatRow);
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

Align all braces with their corresponding block.

Lines containing braces must contain only the brace.

Correct:

```csharp
public void Execute()
{
    Process();
}
```

Incorrect:

```csharp
public void Execute() {
    Process();
}
```

Use braces for every block structure, regardless of the number of statements it contains.

This requirement applies to control flow structures, methods, constructors, and other blocks.

## Line Length

Source code lines must not exceed the maximum line length defined by the Amplivec coding standards.

When a condition, method invocation, expression, declaration, or other statement would exceed that limit, split it across multiple lines while preserving readability and indentation.

Apply the blank-line rules for multiline statements to the resulting statement.

Do not keep an excessively long condition on a single line.

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
