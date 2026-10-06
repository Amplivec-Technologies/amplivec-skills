# HTML Formatting Conventions

Apply these rules to HTML documents and partials. Format the edited region consistently with its surrounding markup. Keep formatting separate from content, component design, and application behavior.

Use the effective IDE formatter profile defined in the main skill together with the explicit multiline-text and sentence-boundary requirements below. The delivered document must satisfy those requirements and be stable under Format Document. If the formatter removes the required layout, report and resolve the profile conflict rather than discarding a rule or repeatedly alternating between incompatible layouts.

## Indentation and Document Hierarchy

- Use two spaces for each markup nesting level by default. Follow a different indentation only when required by the verified project formatter profile. This HTML-specific rule takes precedence over the skill's general tab-indentation rule.
- Indent a child that begins on a new line one level deeper than its parent. Repeated siblings use the same indentation level.
- Align a standalone closing tag with the logical indentation level of its opening element, not with a wrapped attribute or text continuation.
- When an inline fragment contains nested tags, indent subsequent lines for their logical nesting depth, not to the column where a tag happens to appear inside a line of text.
- In complete documents, keep the doctype, `html`, `head`, `body`, and their closing tags at the document-shell indentation level. Indent the contents of `head` and `body` by two spaces. Partials begin with their own root element at column zero; do not add a document shell to a partial.
- Embedded programming languages retain their own applicable conventions. Markup indentation does not redefine JavaScript, CSS, or other language rules.

## Structural Elements and Independent Siblings

- Expand sections, forms, navigation groups, lists, and other substantial component trees across multiple lines. Do not generate a complete nested component on one compressed line.
- By default, start independent navigation links, form-field groups, controls, list items, and accordion entries on separate source lines. Use the compact-fragment exceptions below for small text, icon, or labeling units rather than imposing a universal line break at every tag boundary.
- For an expanded structural container, prefer its opening tag on its own line, its children on indented lines, and its closing tag on a separate aligned line.
- Distinguish an independent control from phrasing inside one control: a currency prefix and a monetary input are separate children of a field wrapper, while a checkbox wrapped by its label is one labeling unit.
- Keep neighboring structural groups visually distinct. Compact text or icon fragments are exceptions within a component, not a reason to compress the surrounding form, list, navigation, or section.

Correct:

```html
<div class="field">
  <label for="amount">Importe</label>
  <div class="money-input">
    <span aria-hidden="true">$</span>
    <input id="amount" name="amount" type="text" inputmode="decimal"
      aria-describedby="amount-error" required>
  </div>
  <p id="amount-error" hidden></p>
</div>
```

Incorrect:

```html
<div class="field"><label for="amount">Importe</label><div class="money-input"><span aria-hidden="true">$</span><input id="amount" name="amount" type="text" inputmode="decimal" aria-describedby="amount-error" required></div><p id="amount-error" hidden></p></div>
```

Apply the same structural separation to short components, not only to lines that exceed a length limit:

```html
<nav aria-label="Navegación">
  <a href="#services">Servicios</a>
  <a href="#questions">Preguntas frecuentes</a>
</nav>
```

## Text and Inline Content

- Keep short text-only elements on one line when they fit the effective limit and do not require a break between sentences. Headings, labels, captions, options, simple list items, links, and buttons do not automatically require three lines.
- Keep inline emphasis, units, short spans, and links with their surrounding text. The presence of a child element alone is not sufficient to require a multiline block. Inline phrasing such as `strong` or `a` may wrap within an expanded text container without moving each inline opening and closing tag onto separate lines; preserve the rendered phrase and significant spaces.
- Preserve deliberate `br` elements inside text. A source-code line wrap is not a rendered line break: do not add `br` elements to enforce source formatting.
- When a text container such as `p`, a heading, or a text-bearing `li` exceeds the effective line-length limit and must span multiple lines, put the opening tag on its own line, then a source newline, then the text indented one level. End with another source newline and the closing tag aligned with the opening tag. Do not leave the first text beside the opening tag or the final text beside the closing tag in that expanded container.
- Wrap long sentences at word boundaries within the expanded container. Continuation lines remain at the content's indentation level rather than forming a staircase. Nested inline content uses its own nesting depth.
- Apply the same expanded layout when the sentence-boundary rule below makes a text container multiline, even if line length alone would not require expansion.

Compact text and inline content:

```html
<h2>Conocé las alternativas</h2>
<label for="amount">Importe <span class="unit">ARS</span></label>
<p>Elegí la opción <strong>más conveniente</strong>.</p>
<h2>Un proceso simple<br>de principio a fin.</h2>
```

Expanded text that wraps across lines:

```html
<p>
  Completá la información solicitada para conocer las alternativas disponibles
  y elegir cómo continuar.
</p>
```

Text on its own indented line:

```html
<p class="notice" id="form-notice">
  Revisá la información antes de continuar.
</p>
```

Opening and closing tags remain separate when the text contains an inline link:

```html
<p>
  Consultá la información disponible en la
  <a href="politica-de-privacidad.html">Política de Privacidad</a>.
</p>
```

When text inside an inline element wraps, its continuation follows the nesting depth without expanding that phrasing element into a separate block:

```html
<p>
  Podés consultar la <a href="politica-de-privacidad.html">información sobre el tratamiento
    de datos personales</a>.
</p>
```

## Sentence Boundaries in Text

- After every sentence-ending period in ordinary HTML prose, start the following sentence on a new source line, even when both sentences fit within the effective line-length limit. A long sentence may still occupy multiple source lines; never place the next sentence on its final line.
- Use one source newline, not an empty line, a `br`, or an additional paragraph. Under normal HTML whitespace collapsing it renders as the separating space while making the sentences distinct in the source.
- Keep any closing inline tags or closing quotation marks attached to the sentence before starting the next line. For example, break after `.</strong>` or `</strong>.` as appropriate; do not insert a visible space before punctuation.
- For the final sentence of an expanded container, put the container's closing tag on the next line, aligned with its opening tag.
- Interpret periods as sentence boundaries, not arbitrary dot characters. Do not split abbreviations such as `Av.` within a sentence, decimal or thousands separators, URLs, email addresses, file names, attribute values, or code. Do not split the dots of an ellipsis individually.
- Preserve whitespace-sensitive content such as `pre`, `code`, `textarea`, or text displayed with whitespace-preserving CSS. Do not apply this prose rule where a source newline would change the rendered content.

Example following the deCauciones privacy-policy edits:

```html
<p>
  Los datos se utilizan para responder la solicitud y acompañar la gestión de una eventual
  contratación.
  El primer contacto lo realiza el <strong>equipo de deCauciones, en nombre de deCauciones,
    por WhatsApp o email</strong>.
  Esos canales se utilizan para continuar la consulta que iniciaste.
</p>
<p>
  El DNI permite identificar al interesado y preparar la validación posterior.
  <strong>La web no realiza por sí misma evaluación crediticia, scoring ni decisiones
    automatizadas de aprobación.</strong>
  La validación corresponde al circuito de la aseguradora.
</p>
```

## Icons, Mixed Content, and Compact Fragments

- For expanded SVG markup, place children such as `use` on their own indented lines. A short empty `use` element can retain its opening and closing tags together.
- In an expanded button, link, or label with an icon, begin the content on the line after the complete opening tag and indent it one level. Close the outer control on its own line at its opening indentation level.
- Compact inline fragments may retain adjacent tags, including short nested spans, icon wrappers, and a checkbox with its label content. Adjacent inline closing tags are allowed within such a fragment; not every closing tag needs its own line.
- Small decorative groups of empty inline elements may remain compact. Do not use this exception to combine independent form fields or navigation items.
- Do not infer a blanket rule from a remaining compact boundary such as a container close followed immediately by a link or button. For new structural markup, prefer the expanded layout; preserve an existing compact inline fragment when it is coherent or whitespace-sensitive.

Expanded control with an icon:

```html
<button class="button" type="button" aria-expanded="false"
  aria-controls="details-panel">
  Ver detalles
  <svg class="icon" aria-hidden="true">
    <use href="icons.svg#arrow"></use>
  </svg>
</button>
```

Permitted compact labeling and decoration units:

```html
<label><input name="consent" type="checkbox"><span>Acepto continuar</span></label>
<div class="decoration"><span></span><span></span></div>
```

## Attributes and Opening-Tag Wrapping

- Keep short opening tags on one line. Multiple complete attributes may share a line.
- When an opening tag is too long, wrap between complete attributes. Keep the tag name and as many readable attributes as appropriate on the first line; indent continuation lines by two additional spaces.
- Keep the terminating `>` on the final attribute line. Do not require a standalone `>` line or one attribute per line.
- In inline text, a tag name may end a line and its attributes continue on the next line when that is the natural wrap point. Account for the nested element's indentation level.
- Use double quotes for attribute values and no spaces around `=`. Preserve attribute order unless changing it is explicitly requested or necessary for another requirement.
- Preserve valueless HTML boolean attributes such as `required`, `disabled`, `hidden`, and `novalidate`. Preserve explicit ARIA values and existing `data-*` values or marker attributes; do not treat every attribute as an HTML boolean.
- Keep attribute values, URLs, and other indivisible tokens intact. Do not split a quoted value merely to satisfy a character count if that would change its value or meaning.
- Use the effective formatter's wrapping setting, including its verified default when the project has no override. For the built-in VS Code HTML profile, `html.format.wrapLineLength` defaults to 120. Treat it as a wrapping target rather than a hard maximum for indivisible tokens. Do not infer a different limit from the apparent width of neighboring lines. Structural readability still requires expanding dense markup even without a numeric limit.

```html
<input id="email" name="email" type="email" autocomplete="email"
  aria-describedby="email-error" required>
```

## Empty and Void Elements

- Keep short empty paired elements compact: `<p id="message" hidden></p>`, `<span data-label></span>`, and `<div data-include="partials/header.html"></div>`.
- Do not insert blank text lines inside elements that intentionally have no content.
- Use HTML void-element syntax for elements such as `input`, `meta`, `link`, and `br`: no closing tag and no XHTML-style trailing slash for ordinary HTML.
- Keep explicit closing tags for non-void elements, including empty elements and `script` elements with `src`. Do not turn an empty `div`, `span`, or `script` into a self-closing HTML tag.
- Preserve the syntax and case-sensitive names of embedded foreign content. For example, an explicitly paired SVG `use` is not an HTML void element.

## Blank Lines and Document Boundaries

- Separate major sections and substantial logical groups with one blank line where it clarifies the document structure.
- Keep related field children, repeated navigation links, list items, and accordion entries consecutive. Do not add a blank line between every element just because some elements span multiple source lines.
- Within ordinary content containers, do not add empty lines immediately after the opening tag, immediately before the closing tag, or between successive structural closing tags solely for formatting. The document-shell boundaries below are an explicit exception.
- For a complete document, keep the doctype and opening `html` tag consecutive. Separate the opening `html` tag from `head`, `head` from `body`, and the closing `body` from the closing `html` with one blank line.
- Do not apply programming-language scope or multiline-statement blank-line rules mechanically to HTML tags.

```html
<!DOCTYPE html>
<html lang="es">

<head>
  <meta charset="UTF-8">
  <title>Ejemplo</title>
</head>

<body>
  <main>
    <h1>Información principal</h1>
  </main>
</body>

</html>
```

## Preserve Meaning When Formatting

- Preserve element order, hierarchy, attributes, text, and behavior unless a separate requirement authorizes changing them.
- Preserve meaningful spaces and adjacency around inline spans, links, icons, and punctuation. A newline can introduce a rendered space; joining lines can remove one. Preserve intentionally contiguous fragments such as parts of a word rather than blindly separating them.
- Preserve whitespace-sensitive content, including `pre`, `textarea`, and code or text whose whitespace is significant. Do not reflow it as ordinary prose.
- Do not remove fallbacks, controls, messages, or metadata as a formatting operation. Do not rewrite wording to shorten a line.
- Follow the explicit repository policy and verified formatter/save behavior for end-of-file newlines, including preserving their absence when that is the established result. Check file boundaries as part of the main skill's formatting verification; do not infer a universal newline rule from an unrelated example.

Before finishing an HTML edit, inspect the expanded hierarchy, attribute continuations, inline exceptions, and text boundaries. Check that multiline text containers have separate aligned opening and closing tags, indented content, and source breaks between sentences. Confirm that the formatting improves readability without introducing a content or behavior change. Run the matching Format Document operation again and verify that it preserves these rules without changes; report when that check could not be performed or a formatter-profile conflict remains.
