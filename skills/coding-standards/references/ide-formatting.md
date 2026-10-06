# IDE Formatter Compatibility and Verification

Use this workflow for source code and other formattable project files, including HTML partials, CSS, JavaScript modules, C#, Razor, JSON, JSONC, XML, YAML, and Markdown when a formatter is available. Limit the files to the requested scope. Do not recursively rewrite dependencies, generated output, vendored code, or skill submodules. For generated files such as lockfiles, preserve their generator's contract and only format them when explicitly in scope.

## Required Result

Treat the project's effective format-and-save operation as `F`. The delivered file must already satisfy `F(file) = file`, including indentation, spaces, line breaks, encoding, line endings, and the presence or absence of a final newline.

Apply the conventions and formatting before presenting the work as complete. A user should be able to commit that result and subsequently run **Format Document** and save without producing a new diff. A commit is a possible manual verification baseline, not a prerequisite or authorization for the agent to commit.

## Resolve the Formatter Profile

Before choosing layout rules:

1. Identify each file's language mode, including embedded languages and dialects. An `.mjs` file is JavaScript; Razor requires Razor-aware formatting rather than a plain HTML formatter.
2. Inspect applicable `.editorconfig` files and their hierarchy, repository formatter configuration, workspace and language-specific editor settings, and existing formatting commands. Check what the selected provider actually supports; not every VS Code formatter consumes EditorConfig.
3. In VS Code, identify the provider selected by **Format Document With...**, `editor.defaultFormatter`, and language overrides. Account for `editor.detectIndentation`, the document's effective tab size and insert-spaces setting, and language formatting options. Do not assume an installed extension is the selected provider.
4. In Visual Studio Community, identify the active language formatter and the effective settings from EditorConfig and **Tools > Options > Text Editor**. Use **Edit > Advanced > Format Document** / **Ctrl+K, Ctrl+D** as the target operation.
5. Include formatting-related save behavior: trailing whitespace, final newlines, line endings, and encoding. Distinguish it from code actions such as organizing imports, applying fixes, or Code Cleanup, which can go beyond formatting.
6. When the user supplies a before/after formatter diff, treat it as direct evidence for that project and language. Infer concrete rules from it and verify against the selected provider. Do not generalize a local setting to every Amplivec project.

Match the provider version and options when using a command-line equivalent. Do not substitute Prettier for the built-in VS Code providers, or assume `dotnet format` has the same scope as a document-format command. For C#, use a verified whitespace-formatting equivalent with the same solution and EditorConfig settings; avoid unrelated analyzer fixes.

If both IDEs are used on the same files, their effective profiles must agree on those files. If they produce different output, identify the conflicting settings and resolve the intended shared profile with the user. Do not claim universal compatibility with arbitrary extensions, versions, or personal settings. Do not disable formatting, add ignore directives, or silently change project/user settings to conceal a mismatch.

## VS Code Web Formatting Profile

Use these concrete rules when the built-in web formatters are the project's target. The deCauciones staged-versus-Format-Document comparison provides examples of this profile; its EOF behavior is recorded separately below. Recheck the effective options if another provider or project configuration is active.

### CSS

- `css.format.newlineBetweenSelectors: true`: one comma-separated selector per line, at the same indentation level. Keep each comma at the end of its selector line.
- `css.format.newlineBetweenRules: true`: one blank line between rules, including nested rules.
- `css.format.spaceAroundSelectorSeparator: false`: no surrounding spaces for `>`, `+`, and `~` in selectors. Descendant spaces remain meaningful and must be preserved.
- Use multiline declaration blocks, one declaration per line, a space after the declaration colon, and semicolons. Place the opening brace on the last selector line and align the closing brace with the rule.
- Use the document's effective indentation; the Amplivec CSS fallback and the observed deCauciones profile use tabs with a visual width of four spaces.
- Preserve property order and values. A selector list is not a declaration value: do not split `font-family` or remove required spaces in `calc()` by applying selector rules with a global text replacement.

Example for this profile:

```css
.field>label,
.field>legend {
	display: block;
	color: var(--color-text-primary);
}

@media (min-width: 40rem) {
	.field input:checked+label,
	.field input:focus-visible+label {
		border-color: var(--color-focus);
	}
}
```

### HTML and Partials

- Use the detailed rules in the skill's HTML reference together with the formatter's actual output.
- The built-in profile uses `html.format.wrapLineLength: 120` by default. Use it as the formatter's wrapping target, not a universal hard maximum or a reason to split attribute values and URLs.
- With `html.format.wrapAttributes: "auto"`, wrap between whole attributes when needed. Retain the provider's continuation indentation and closing angle-bracket placement.
- With `html.format.indentInnerHtml: false`, keep `head` and `body` at the document-shell level. The observed project uses two spaces for children and continuation levels.
- Apply Amplivec's explicit multiline-text rule: when a text container exceeds the effective line-length limit, put the opening tag, the indented text, and the aligned closing tag on separate lines. Use that expanded layout also when sentence separation makes the container multiline.
- Start each following sentence on a new source line after a sentence-ending period, without inserting `br` or a new paragraph. A long sentence can wrap further. Keep closing inline tags with the sentence and preserve non-sentence dots in abbreviations, numbers, URLs, and email addresses.
- Inline `a`, `strong`, or `span` elements can remain integrated into the text, including wrapped text at their logical nesting depth. Do not introduce visible spaces between an inline closing tag and attached punctuation.
- Verify that Format Document preserves both expanded containers and sentence breaks. These are explicit Amplivec requirements, not optional layouts to discard in favor of a formatter's default. If the provider joins sentences or returns text to the opening/closing tag line, report the profile conflict and resolve the intended configuration with the user rather than claiming compatibility or silently changing settings.
- Preserve intentional inline spaces, contiguous fragments, and whitespace-sensitive content. Do not treat a semantic text change as acceptable merely because a formatter produced it; inspect and resolve the cause.

### JavaScript, TypeScript, and C#

- Retain the skill's language-appropriate naming and control-flow requirements. A formatter does not necessarily add required braces or meaningful blank-line separation for you.
- Use the effective provider for continuation indentation, parentheses, brace placement, object literals, and punctuation. Keep 1TBS for JavaScript/TypeScript and Allman for C# as fallbacks where the configured profile does not override them.
- Do not force the HTML wrapping target or CSS selector layout onto programming-language expressions.

### JSON and Other Formattable Files

- Include formattable configuration files in a repository-wide formatting task, not just application source. JSON and JSONC use two-space indentation in the observed VS Code profile; use the effective profile elsewhere.
- Preserve JSON property order, values, and validity; do not add comments or trailing commas to strict JSON.
- Use the correct provider for YAML, XML, Markdown, and other supported formats. Preserve significant indentation and Markdown hard line breaks. Do not run all formats through a single generic whitespace normalizer.

## File Boundaries and Save Behavior

- Check trailing whitespace, LF versus CRLF, encoding/BOM, and the final newline independently of visible indentation.
- Honor explicit project policy and the verified formatter/save result. Do not universally add or remove a final newline because of a coding preference.
- In the observed deCauciones comparison, formatting removed the final newline from the changed CSS, JSON, and HTML files. Preserve that no-final-newline result for those files when reproducing this profile. This observation does not establish a repository-wide rule for JavaScript, Markdown, unchanged files, or other projects.
- Consider provider-specific final-newline options and save settings together. For example, a disabled insert-final-newline option does not, by itself, prove that an existing newline will be removed.
- If a patching or generation tool inserts a final newline automatically, check the resulting bytes and reconcile them with the verified profile before claiming equivalence.

## Verification Workflow

1. Record the existing staged and unstaged state before editing. When the index must be preserved, use read-only index listings or hashes to verify that preservation afterward.
2. Apply the requested coding changes and the language conventions to the in-scope files.
3. Run the actual document formatter or an equivalent proven to match its provider/version/settings, then save with the effective whitespace behavior. Respect the available tools and their editing constraints; a read-only formatting check or temporary-copy comparison is appropriate when in-place execution is unavailable.
4. Review the complete formatted diff for changed text, meaningful whitespace, selector semantics, generated data, and accidental out-of-scope edits.
5. Capture the final file contents and invoke the same operation again. Require exact unchanged contents. Checking only that `F(F(original)) = F(original)` is insufficient if the delivered file still contains the unformatted `original`.
6. In an already-clean workspace, a second format-and-save pass should leave the diff empty. In a dirty or partially staged workspace, compare with the captured pre-pass contents rather than requiring an empty repository diff or resetting user work.
7. Report the provider/profile, files checked, and any unavailable checks. Syntax, build, lint, semantic equivalence, and screenshots cannot substitute for this formatting check.

If no matching formatter is available, follow the concrete verified rules, inspect the output, and disclose the unverified IDE check. Do not invent successful execution of an editor or promise identical results for a provider that was not checked.

## References

- [VS Code CSS formatting](https://code.visualstudio.com/docs/languages/css#_formatting)
- [VS Code HTML formatting](https://code.visualstudio.com/docs/languages/html#_formatting)
- [Visual Studio EditorConfig and Format Document](https://learn.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options?view=visualstudio)
