# Blogger Component Configuration Master Rule

## 1. PURPOSE

This rule is mandatory for every Blogger XML theme development task.

Every visual or user-facing component added to the theme MUST have a defined configuration strategy.

No component may be implemented as an isolated hard-coded design block.

The architecture must separate:

1. Blogger Theme Designer configuration
2. CSS design tokens
3. Component markup
4. Component behavior
5. Blogger widget configuration

---

# 2. COMPONENT CONFIGURATION REQUIREMENT

Whenever Antigravity IDE creates, modifies, or refactors a component, it MUST first determine:

* What can be customized by the user?
* Which values should be exposed through Blogger Theme Designer?
* Which values are structural constants?
* Which values belong to CSS custom properties?
* Which values belong to Blogger widget settings?
* Which values belong to JavaScript configuration?

The component is NOT considered complete until this mapping exists.

---

# 3. COMPONENTS COVERED BY THIS RULE

This rule applies to ALL major and minor visual components, including:

* Logo
* Header
* Top Bar
* Navigation
* Mobile Navigation
* Search
* Theme Switcher
* Hero
* Featured Post
* Slider
* Post Grid
* Post Card
* Post Thumbnail
* Post Title
* Post Meta
* Labels
* Buttons
* Read More Button
* Sidebar
* Sidebar Widgets
* Popular Posts
* Labels Widget
* Profile Widget
* Search Widget
* Advertisement Areas
* Download Box
* Social Follow
* Social Share
* Table of Contents
* Reading Progress
* Reading Time
* Font Resizer
* Breadcrumbs
* Author Box
* Comments
* Pagination
* Footer
* Footer Columns
* Copyright
* Any future component

---

# 4. COMPONENT CONFIGURATION MATRIX

Before implementing a component, create an internal configuration matrix:

| Component     | Configurable                               | Configuration Source             |
| ------------- | ------------------------------------------ | -------------------------------- |
| Logo          | Color, size, max dimensions                | Theme Designer / CSS             |
| Header        | Background, border, shadow                 | Theme Designer / CSS             |
| Navigation    | Color, hover color, spacing, typography    | Theme Designer / CSS             |
| Hero          | Colors, typography, radius, overlay        | Theme Designer / CSS             |
| Slider        | Visual settings + behavior                 | CSS + data attributes / widget   |
| Post Card     | Background, radius, shadow, spacing        | Theme Designer / CSS             |
| Sidebar       | Width, spacing, background, widget styling | CSS / Theme Designer             |
| Footer        | Background, text, links, spacing           | Theme Designer / CSS             |
| Typography    | Fonts, weights, base size                  | Theme Designer                   |
| Global Colors | Brand/background/text colors               | Theme Designer                   |
| Blog Widgets  | Content and widget options                 | Blogger Layout / Widget settings |

If a property cannot reasonably be exposed through Blogger Theme Designer, it MUST be documented as a structural or runtime constant.

---

# 5. BLOGGER THEME DESIGNER VARIABLES

Use native Blogger `<Variable>` declarations only where supported by the target Blogger implementation.

Do NOT invent unsupported Blogger variable types.

Before adding a variable type, verify that it is actually supported by Blogger.

Never assume that a generic CSS variable type such as:

* length
* boolean
* string
* shadow
* ratio

is automatically supported by Blogger Theme Designer.

If unsupported, keep the value as a CSS custom property or another appropriate configuration mechanism.

---

# 6. COLOR CONFIGURATION

Major user-facing colors SHOULD use Blogger Theme Designer color variables where practical.

Example:

```xml
<Variable
  name="primaryColor"
  description="Primary Brand Color"
  type="color"
  default="#035dcd"
  value="#035dcd"/>
```

CSS MUST consume the variable through a centralized token:

```css
:root {
  --primary-color: $(primaryColor);
}
```

Components MUST use:

```css
color: var(--primary-color);
```

instead of repeatedly using:

```css
color: #035dcd;
```

---

# 7. TYPOGRAPHY CONFIGURATION

Global typography SHOULD use Blogger Theme Designer font variables where supported.

Example:

```xml
<Variable
  name="bodyFont"
  description="Body Font"
  type="font"
  default="normal normal 16px 'Tajawal', sans-serif"
  value="normal normal 16px 'Tajawal', sans-serif"/>
```

Then:

```css
:root {
  --body-font: $(bodyFont);
}
```

Use:

```css
body {
  font: var(--body-font);
}
```

Headings SHOULD have a separate typography token where appropriate.

---

# 8. CSS CUSTOM PROPERTY LAYER

All reusable component values MUST pass through centralized CSS tokens whenever practical.

Example:

```css
:root {
  --primary-color: $(primaryColor);
  --secondary-color: $(secondaryColor);
  --accent-color: $(accentColor);

  --bg-color: $(bodyBackground);
  --surface-color: $(surfaceColor);

  --text-color: $(textColor);
  --muted-color: $(mutedTextColor);
  --border-color: $(borderColor);

  --container-width: 1200px;
  --sidebar-width: 330px;
  --content-gap: 30px;

  --radius: 12px;
  --shadow: 0 4px 12px rgba(0,0,0,.06);
}
```

Components MUST consume these tokens instead of duplicating values.

---

# 9. NO UNNECESSARY HARDCODING

The following MUST NOT be hard-coded repeatedly:

* Brand colors
* Text colors
* Background colors
* Border colors
* Font families
* Global typography
* Global radii
* Global shadows
* Container width
* Sidebar width
* Repeated spacing values
* Repeated component dimensions

Bad:

```css
.logo {
  color: #035dcd;
}

.button {
  background: #035dcd;
}

.link {
  color: #035dcd;
}
```

Good:

```css
.logo {
  color: var(--primary-color);
}

.button {
  background: var(--primary-color);
}

.link {
  color: var(--primary-color);
}
```

---

# 10. STRUCTURAL CONSTANTS

Not every CSS value needs a Theme Designer control.

Structural implementation values MAY remain CSS constants when exposing them to the user would create unnecessary complexity.

Examples:

```css
--container-width: 1200px;
--sidebar-width: 330px;
--content-gap: 30px;
```

However:

1. They MUST be centralized.
2. They MUST NOT be duplicated throughout the stylesheet.
3. They MUST be documented.
4. If the user explicitly requests customization of these values, provide an appropriate configuration mechanism.

---

# 11. LOGO RULE

The Logo is a configurable component.

The implementation MUST support, where applicable:

* Logo image
* Logo text
* Logo color
* Logo typography
* Logo size
* Maximum width
* Maximum height
* Alignment
* Mobile behavior

Do not create a logo component with permanent hard-coded colors or dimensions unless they are documented defaults.

---

# 12. HEADER RULE

The Header MUST have a centralized configuration strategy.

At minimum consider:

* Background
* Text color
* Border
* Shadow
* Height
* Logo
* Navigation
* Mobile behavior
* Sticky behavior

Do not scatter header values across unrelated CSS rules.

---

# 13. SIDEBAR RULE

The Sidebar MUST be a native Blogger section when used as a widget zone.

Example architecture:

```xml
<b:section
  id='sidebar-section'
  class='sidebar-section'
  showaddelement='yes'>
</b:section>
```

The Sidebar MUST support Blogger Layout widget management.

Its visual configuration SHOULD be centralized:

```css
.sidebar {
  width: var(--sidebar-width);
}

.sidebar .widget {
  border-radius: var(--radius);
}
```

The user MUST be able to add, remove, reorder, or configure sidebar widgets through Blogger Layout where technically supported.

---

# 14. SLIDER RULE

Never assume that a Blogger `FeaturedPost` widget provides multiple posts.

`FeaturedPost` MUST be treated according to its actual Blogger data context.

For a multi-item slider, use an architecture that actually provides multiple items, such as:

* an appropriate Blogger widget/data source
* multiple configured widget items
* a controlled client-side feed architecture

Do NOT write:

```xml
<b:loop values='data:posts' var='post'>
```

inside a context that does not expose `data:posts`.

Slider behavior MUST be separated from slider presentation.

---

# 15. WIDGET RULE

Every widget zone MUST be implemented as a native Blogger section where appropriate.

Example:

```xml
<b:section
  id='sidebar-section'
  class='sidebar-section'
  showaddelement='yes'>
```

A `<b:section>` MUST NOT contain arbitrary HTML children.

Valid structure:

```xml
<b:section>
  <b:widget .../>
</b:section>
```

HTML wrappers MUST exist outside the `<b:section>` when required.

---

# 16. BLOG1 SAFETY RULE

`Blog1` is a critical Blogger widget.

Never replace, delete, or simplify Blogger's required Blog widget architecture without verifying compatibility.

Before modifying Blog1:

1. Inspect the existing widget.
2. Identify required includables.
3. Preserve mandatory Blogger functionality.
4. Modify only the required presentation layer.
5. Validate the final widget structure.

Do NOT assume that a simplified custom Blog widget is valid.

---

# 17. DARK MODE RULE

Dark mode MUST NOT unnecessarily destroy user-selected Theme Designer brand colors.

Example:

```css
:root {
  --primary-color: $(primaryColor);
}

[data-theme="dark"] {
  --primary-color: $(primaryColor);
}
```

Dark mode SHOULD primarily modify surface/system tokens:

```css
[data-theme="dark"] {
  --bg-color: #121212;
  --surface-color: #1e1e1e;
  --text-color: #f1f1f1;
  --muted-color: #adb5bd;
  --border-color: #343a40;
}
```

Do not replace the user's selected brand color unless there is a documented accessibility requirement.

---

# 18. JAVASCRIPT CONFIGURATION

JavaScript MUST NOT directly contain duplicated design values.

Bad:

```javascript
const interval = 5000;
```

when the value is intended to be configurable.

Prefer a controlled configuration layer:

```html
<div
  class='hero-slider'
  data-interval='5000'>
</div>
```

or a centralized configuration object.

IMPORTANT:

Do NOT assume `$(variableName)` works directly inside HTML attributes.

Use Blogger-supported expression mechanisms only when they are actually available.

If a Theme Designer variable cannot safely cross into HTML/JS, keep the runtime value in CSS or another verified Blogger-supported mechanism.

---

# 19. COMPONENT COMPLETION CHECK

A component is considered COMPLETE only when all of the following are true:

[ ] Component has a unique semantic structure.

[ ] Component has centralized CSS tokens.

[ ] User-facing colors are configurable where appropriate.

[ ] Typography is configurable where appropriate.

[ ] Repeated dimensions are centralized.

[ ] Responsive behavior is defined.

[ ] Dark mode behavior is defined when applicable.

[ ] Blogger widget integration is valid when applicable.

[ ] Blogger Layout management works when applicable.

[ ] JavaScript does not contain unnecessary hard-coded design values.

[ ] No unsupported Blogger syntax is introduced.

[ ] XML remains well-formed.

[ ] Existing Blogger functionality is preserved.

---

# 20. MANDATORY WORKFLOW FOR ANTIGRAVITY IDE

Whenever the user requests:

"Add a component"

or

"Create a component"

or

"Redesign a component"

Antigravity MUST follow this order:

### STEP 1 — INSPECT

Inspect the existing XML, CSS, JavaScript, sections, widgets, variables, and includables.

### STEP 2 — IDENTIFY CONFIGURATION

Determine all user-facing properties of the component.

### STEP 3 — CLASSIFY

Classify each property as:

A. Blogger Theme Designer variable

B. CSS custom property

C. Blogger widget setting

D. Runtime JavaScript configuration

E. Structural constant

### STEP 4 — IMPLEMENT

Implement the component using the appropriate configuration layer.

### STEP 5 — CONNECT

Connect the component to centralized CSS tokens and Blogger configuration.

### STEP 6 — VALIDATE

Check:

* XML validity
* Blogger syntax
* Theme Designer compatibility
* Widget structure
* Responsive behavior
* Dark mode
* Accessibility
* No unnecessary hard-coded values

### STEP 7 — REPORT

Report:

```text
COMPONENT CONFIGURATION AUDIT

Component:
Configuration:
Theme Designer variables:
CSS tokens:
Widget settings:
JavaScript settings:
Structural constants:
Responsive behavior:
Dark mode:
Validation:
Status:
```

---

# 21. GLOBAL DESIGN SYSTEM RULE

The theme MUST behave as a design system rather than a collection of independent components.

All components SHOULD inherit from shared tokens:

```text
Theme Designer
      ↓
Blogger Variables
      ↓
CSS Tokens
      ↓
Components
      ↓
Responsive / Dark Mode
```

Do not create independent color systems, typography systems, or spacing systems for individual components unless there is a specific architectural reason.

---

# 22. PRIORITY RULE

When rules conflict, use this priority:

1. Blogger XML validity
2. Blogger runtime compatibility
3. Native Blogger widget compatibility
4. Theme Designer compatibility
5. Accessibility
6. SEO
7. Performance
8. Design customization
9. Visual decoration

Never sacrifice Blogger XML validity to obtain a visual feature.

Never sacrifice native Blogger functionality merely to simplify CSS or HTML.

---

# 23. FINAL PROHIBITION

Antigravity MUST NOT add a new visual component and leave its major design values scattered as hard-coded CSS.

Before declaring implementation complete, it MUST answer:

"How can the user configure this component?"

If the answer is:

"Only by editing the XML/CSS manually"

then the component MUST be reviewed again unless the value is explicitly classified as a structural constant.

END OF RULE
