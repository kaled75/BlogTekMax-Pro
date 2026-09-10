# BLOGGER XML SAFETY — TEKMAX MASTER RULE

## STATUS

This is a mandatory safety and validation rule for the Blogger XML theme.

The AI Agent MUST follow this rule whenever creating, editing, refactoring, formatting, or reviewing a Blogger XML template.

The primary objective is:

> NEVER generate invalid XML or Blogger-incompatible template syntax.

---

# 1. BLOGGER XML IS NOT NORMAL HTML

Treat the theme file as a strict XML document with Blogger-specific extensions.

DO NOT treat the template as ordinary HTML.

Before making changes, consider:

* XML well-formedness
* XML escaping
* Blogger namespaces
* Blogger template syntax
* Blogger conditional expressions
* Blogger sections
* Blogger widgets
* Blogger-specific attributes
* XML element closure
* XML attribute validity

---

# 2. XML VALIDITY IS MANDATORY

Every modification MUST preserve XML well-formedness.

The final file MUST satisfy:

```text
XML declaration valid
Namespaces valid
All elements properly nested
All opened elements properly closed
All attributes quoted
No illegal characters
No undeclared entities
No malformed comments
No invalid XML syntax
```

Never declare a task complete if XML validation fails.

---

# 3. XML SPECIAL CHARACTERS

Always correctly escape XML-sensitive characters when they appear in XML text or attribute values.

Required mappings:

```text
&   → &amp;
<   → &lt;
>   → &gt;
"   → &quot;   when required
'   → &apos;   when required
```

CRITICAL:

Never write:

```text
&nbsp;
```

because `nbsp` is NOT a predefined XML entity.

Use:

```text
&#160;
```

or:

```text
&#xA0;
```

when a non-breaking space is actually required.

The same principle applies to other HTML entities that are not valid XML entities.

---

# 4. AMPERSAND SAFETY

Every literal `&` inside XML content MUST be evaluated.

Invalid:

```xml
<a href="page.html?a=1&b=2">
```

Valid:

```xml
<a href="page.html?a=1&amp;b=2">
```

Never introduce an unescaped `&`.

---

# 5. ELEMENT CLOSURE

Every XML element MUST be correctly closed.

Invalid:

```xml
<input type="text">
```

Valid:

```xml
<input type="text" />
```

For normal non-void XML elements:

```xml
<div>
    ...
</div>
```

Do not rely on HTML's optional closing-tag rules.

XML requires explicit structure.

---

# 6. SELF-CLOSING ELEMENTS

Elements that contain no child content MUST use XML-compatible self-closing syntax where appropriate.

Example:

```xml
<input type="search" />
```

```xml
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

```xml
<link rel="preconnect" href="https://fonts.googleapis.com" />
```

However, Blogger-specific elements MUST follow Blogger's supported syntax.

Do not blindly self-close Blogger elements that require explicit closing tags.

---

# 7. ATTRIBUTE SAFETY

Every XML attribute MUST:

1. Have a valid name.
2. Have a quoted value.
3. Not contain unescaped XML characters.
4. Not contain malformed Blogger expressions.

Correct:

```xml
<div class="post-card" data-id="123">
```

Incorrect:

```xml
<div class=post-card>
```

Incorrect:

```xml
<div title="Tom & Jerry">
```

Correct:

```xml
<div title="Tom &amp; Jerry">
```

---

# 8. XML COMMENTS

XML comments MUST NOT contain:

```text
--
```

inside the comment body.

Do not create malformed comments.

Correct:

```xml
<!-- Header component -->
```

Avoid comments containing invalid XML comment sequences.

---

# 9. BLOGGER NAMESPACES

Never remove or modify required Blogger namespaces without a valid reason.

Before editing the root element, verify all required namespaces.

Typical Blogger template namespaces may include:

```xml
xmlns='http://www.w3.org/1999/xhtml'
xmlns:b='http://www.google.com/2005/gml/b'
xmlns:data='http://www.google.com/2005/gml/data'
xmlns:expr='http://www.google.com/2005/gml/expr'
```

Do not invent namespaces.

Do not remove a namespace that is still referenced by the template.

---

# 10. BLOGGER TEMPLATE ELEMENTS

Treat Blogger-specific elements as special syntax.

Examples include:

```text
<b:skin>
<b:section>
<b:widget>
<b:includable>
<b:include>
<b:if>
<b:else>
<b:loop>
<b:eval>
<b:with>
```

Do not convert Blogger elements into ordinary HTML.

Do not replace Blogger syntax with unsupported alternatives.

---

# 11. b:IF / b:ELSE SAFETY

Every conditional block MUST be structurally valid.

Example pattern:

```xml
<b:if cond='...'>
    ...
<b:else />
    ...
</b:if>
```

Do not create:

```text
<b:if> without </b:if>
```

Do not create orphan:

```text
<b:else />
```

Every `b:else` MUST belong to a valid `b:if`.

After modifications, count and verify conditional structures.

---

# 12. B:LOOP SAFETY

Every:

```xml
<b:loop>
```

MUST have a matching:

```xml
</b:loop>
```

Do not create malformed nesting.

Example:

```xml
<b:loop values='data:posts' var='post'>
    ...
</b:loop>
```

Verify that loop variables are used consistently.

---

# 13. B:INCLUDE / B:INCLUDABLE SAFETY

Before modifying:

```text
<b:includable>
<b:include>
```

verify:

* Unique IDs
* Correct references
* Correct nesting
* Correct parameters
* No orphan includes
* No duplicate includable IDs where prohibited

Do not remove an includable without checking all references.

---

# 14. BLOGGER SECTIONS

Every `<b:section>` MUST remain Blogger-compatible.

Before modifying a section verify:

* Valid ID
* Valid class
* Correct structure
* Correct maxwidgets where applicable
* Correct showaddelement where applicable
* Valid nested widgets

Do not place arbitrary unsupported structures inside Blogger section definitions.

---

# 15. BLOGGER WIDGETS

Every Blogger widget MUST remain structurally valid.

Before modifying a widget verify:

```text
Widget type
Widget id
Widget title
Widget locked state
Widget version
Includables
Widget settings
```

Do not arbitrarily rename widget IDs.

Do not remove widget settings without checking dependencies.

Do not break existing Blogger widget compatibility.

---

# 16. WIDGET CONFIGURATION SAFETY

When adding widget settings:

1. Verify the setting is supported.
2. Use the correct Blogger syntax.
3. Do not invent unsupported attributes.
4. Do not mix normal HTML attributes with Blogger widget configuration syntax incorrectly.

If native Blogger widget configuration cannot support a requested feature:

DO NOT invent fake Blogger syntax.

Use a safe `<b:skin>` variable or another Blogger-compatible mechanism when appropriate.

---

# 17. B:SKIN SAFETY

All CSS variables intended for Blogger Theme Designer MUST be defined correctly inside:

```xml
<b:skin><![CDATA[
...
]]></b:skin>
```

or the template's established valid `<b:skin>` structure.

Do not place arbitrary XML inside CSS.

Do not break the `<b:skin>` boundaries.

Verify every Blogger `<Variable>` declaration.

Example:

```xml
<Variable name="primaryColor"
          description="Primary Color"
          type="color"
          default="#035dcd"
          value="#035dcd"/>
```

Variable names MUST be unique.

---

# 18. CSS INSIDE XML

CSS must remain XML-safe.

Preferred approach:

```xml
<b:skin><![CDATA[
    .example {
        color: red;
    }
]]></b:skin>
```

When CSS is inside CDATA:

* Do not create `]]>` inside the CSS.
* Keep CSS syntactically valid.
* Do not place invalid XML outside CDATA.

---

# 19. JAVASCRIPT INSIDE XML

JavaScript embedded in XML MUST be XML-safe.

Preferred approach:

```xml
<script type='text/javascript'>
//<![CDATA[
    // JavaScript
//]]>
</script>
```

or use the established valid structure of the existing template.

Do not allow JavaScript operators or strings to accidentally become invalid XML.

Be especially careful with:

```text
&
<
>
```

inside JavaScript.

---

# 20. URL SAFETY

URLs containing query parameters MUST escape ampersands.

Invalid:

```text
https://example.com/?a=1&b=2
```

Inside XML:

```text
https://example.com/?a=1&amp;b=2
```

Do not corrupt URLs while escaping them.

---

# 21. CSS / JS / HTML SEPARATION

Do not solve XML problems by randomly changing valid CSS or JavaScript.

When an XML error occurs:

1. Identify the XML parser problem.
2. Identify the exact line.
3. Fix the XML syntax.
4. Revalidate CSS/JS independently.

Do not introduce unnecessary architectural changes.

---

# 22. NEVER BLINDLY REPLACE TEXT

Before global replacement:

```text
replace all
regex replace
search/replace
```

check whether the target appears inside:

* XML
* HTML
* CSS
* JavaScript
* Blogger expressions
* URLs
* comments
* strings

A replacement that is valid in one context may be invalid in another.

---

# 23. PRESERVE EXISTING FUNCTIONALITY

When fixing XML:

DO NOT unnecessarily change:

* Design
* Layout
* SEO
* JavaScript behavior
* Widget functionality
* Blogger data expressions
* URLs
* IDs
* Classes
* Accessibility
* Performance

The objective of an XML safety fix is:

> Fix the syntax while preserving behavior.

---

# 24. VALIDATION AFTER EVERY STRUCTURAL CHANGE

After modifying any of the following:

```text
<b:if>
<b:else>
<b:loop>
<b:section>
<b:widget>
<b:includable>
<b:include>
<b:skin>
<Variable>
```

perform structural validation.

Do not wait until the end of a large refactor.

---

# 25. MULTI-STAGE VALIDATION

Before declaring the template complete, perform these validation stages.

## STAGE 1 — XML

Verify:

```text
[ ] XML is well-formed
[ ] All tags are closed
[ ] Attributes are quoted
[ ] XML entities are valid
[ ] No undeclared HTML entities
[ ] No unescaped &
[ ] No malformed comments
```

## STAGE 2 — BLOGGER STRUCTURE

Verify:

```text
[ ] Blogger namespaces exist
[ ] b:if structures are valid
[ ] b:else structures are valid
[ ] b:loop structures are valid
[ ] b:section structures are valid
[ ] b:widget structures are valid
[ ] b:include references are valid
[ ] b:includable structures are valid
[ ] b:skin is valid
[ ] Variables are valid
```

## STAGE 3 — CSS

Verify:

```text
[ ] CSS syntax valid
[ ] No broken braces
[ ] No broken media queries
[ ] Blogger variables referenced correctly
```

## STAGE 4 — JavaScript

Verify:

```text
[ ] JavaScript syntax valid
[ ] No accidental XML corruption
[ ] No broken selectors
[ ] No undefined configuration references
```

## STAGE 5 — FUNCTIONAL

Verify:

```text
[ ] Header works
[ ] Navigation works
[ ] Search works
[ ] Dark mode works
[ ] Slider works
[ ] Sidebar works
[ ] Post cards work
[ ] Single post works
[ ] Footer works
[ ] Mobile menu works
```

---

# 26. XML ERROR TRIAGE

When Blogger reports an error such as:

```text
SAXParseException
```

DO NOT guess.

Follow this process:

```text
1. Capture exact error message.
2. Identify line and column if provided.
3. Inspect surrounding XML.
4. Determine whether the error is:
   XML syntax
   Blogger syntax
   CSS
   JavaScript
   Entity escaping
   Tag nesting
5. Apply the smallest safe fix.
6. Validate again.
```

---

# 27. COMMON ERROR PATTERNS

Always check for these known failure patterns.

### Error: Entity not declared

Likely causes:

```text
&nbsp;
&copy;
&hellip;
&times;
```

Solution:

Use valid XML entities or numeric character references.

Example:

```text
&#160;
&#169;
&#8230;
&#215;
```

or the correct escaped representation where appropriate.

---

### Error: Element must be terminated

Likely causes:

```text
<input>
<img>
<meta>
<link>
```

Solution:

Use XML-compatible self-closing syntax where appropriate:

```text
<input />
<img />
<meta />
<link />
```

---

### Error: Attribute value must be quoted

Fix:

```xml
class="example"
```

not:

```xml
class=example
```

---

### Error: EntityRef expected

Likely cause:

Unescaped:

```text
&
```

Fix:

```text
&amp;
```

---

### Error: Mismatched closing tag

Check:

```text
Opening tag
Closing tag
Nesting order
```

Do not simply add random closing tags.

---

# 28. DO NOT USE HTML5 SHORTCUTS

Do not rely on HTML parsing rules.

Avoid assuming that XML will automatically accept:

```html
<br>
<hr>
<img>
<input>
<meta>
<link>
```

Use XML-compatible syntax where required.

---

# 29. ENCODING

The template MUST use UTF-8.

Do not introduce:

* Invalid byte sequences
* Unexpected encoding conversions
* Corrupted Arabic text
* Invisible control characters

Arabic, English, and mixed RTL/LTR content MUST remain intact.

---

# 30. BACKUP BEFORE LARGE REFACTORING

Before a large structural modification:

1. Preserve the original file.
2. Work on a controlled copy when possible.
3. Make changes incrementally.
4. Validate after major stages.

Never overwrite a known-good template with an unvalidated large rewrite.

---

# 31. MINIMAL CHANGE PRINCIPLE

When fixing an error:

> Make the smallest change that solves the problem.

Do NOT rewrite the entire template to fix one XML error.

Do NOT refactor unrelated code during an XML safety fix.

---

# 32. FINAL XML QUALITY GATE

The Agent MUST NOT report:

```text
COMPLETE
```

until:

```text
[ ] XML parser validation passes
[ ] No SAXParseException
[ ] No undeclared entities
[ ] No unescaped XML ampersands
[ ] No malformed tags
[ ] No mismatched tags
[ ] Blogger namespaces valid
[ ] Blogger structural elements valid
[ ] b:if/b:else balanced
[ ] b:loop balanced
[ ] b:includable/include references verified
[ ] b:section structures verified
[ ] b:widget structures verified
[ ] b:skin verified
[ ] Variables verified
[ ] CSS syntax verified
[ ] JavaScript syntax verified
[ ] Existing functionality preserved
```

---

# 33. FINAL INSTRUCTION

For every Blogger XML modification:

```text
PLAN
  ↓
MODIFY
  ↓
VALIDATE XML
  ↓
VALIDATE BLOGGER STRUCTURE
  ↓
VALIDATE CSS
  ↓
VALIDATE JS
  ↓
VERIFY FUNCTIONALITY
  ↓
REPORT RESULT
```

Never skip validation.

Never claim XML validity without actually validating the resulting file.

If a validation tool is available, use it.

If validation cannot be performed, explicitly state that validation could not be performed instead of claiming success.

---

# PRIORITY

When conflicting requirements occur, follow this priority:

1. XML well-formedness
2. Blogger compatibility
3. Existing functionality
4. Accessibility
5. SEO
6. Performance
7. Theme Designer configurability
8. Visual changes
9. Code refactoring

NEVER sacrifice XML or Blogger compatibility for visual convenience.
