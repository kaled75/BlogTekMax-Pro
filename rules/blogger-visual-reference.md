# Blogger Theme — Visual Reference Implementation Rule

I will provide a reference image of a Blogger theme/interface.

Use the image **ONLY as a visual design reference**.

## Objective

Build a completely original, production-ready Blogger XML theme that is **visually inspired by the reference image** in:

* Overall layout
* Visual hierarchy
* Header structure
* Navigation style
* Hero/featured area
* Post card composition
* Sidebar arrangement
* Footer structure
* Spacing rhythm
* Border radius
* Shadows
* Typography hierarchy
* Responsive behavior
* General modern aesthetic

## IMPORTANT — DO NOT COPY

Do NOT copy or reproduce:

* Source code
* HTML structure exactly
* CSS code
* Class names
* IDs
* JavaScript
* SVG code
* Images
* Logos
* Text/content
* Exact proprietary visual assets
* Pixel-perfect implementation

Do not create a clone.

Instead, **analyze the visual principles of the reference and create an original implementation** suitable for BlogTekMax Pro.

---

# Blogger Architecture Requirements

The result MUST be a real functional Blogger XML theme, not a static HTML landing page.

Use:

* Native Blogger XML
* `<b:skin>`
* Native Blogger Theme Designer variables
* `<b:section>`
* `<b:widget>`
* Native `Blog1`
* Native Blogger data expressions
* Responsive CSS
* Semantic HTML
* Accessible markup
* SEO-friendly structure

The native Blogger blog engine MUST remain functional.

Do NOT hide or disable `Blog1`.

---

# Theme Designer Requirement

Every major visual component MUST be designed so its important visual properties can be controlled from:

Blogger → Theme → Customize

Create appropriate native Blogger `<Variable>` definitions inside `<b:skin>`.

Use only verified Blogger Theme Designer variable types:

* `color`
* `font`

Do NOT invent unsupported Theme Designer variable types such as:

* `length`
* `string`
* `boolean`
* `shadow`
* `ratio`

Structural values that cannot be exposed through the native Theme Designer should remain controlled through centralized CSS custom properties.

---

# Components That MUST Be Configurable

Create Theme Designer controls for all important visual elements.

## Global

* Primary color
* Secondary color
* Accent color
* Background color
* Surface/card color
* Text color
* Muted text color
* Border color
* Link color
* Body font
* Heading font

## Header

* Header background
* Header border
* Logo color
* Navigation text color
* Navigation hover color

## Logo

The logo system must support:

* Blogger Header widget
* Image logo
* Text logo fallback
* Responsive sizing
* Maximum width
* Proper alignment

## Navigation

Support:

* LinkList widget
* Desktop navigation
* Mobile navigation
* Active/hover states
* Responsive menu

## Hero / Featured Area

Support:

* FeaturedPost widget
* Featured image
* Title
* Description/snippet
* Category/label
* CTA where appropriate

Do not assume FeaturedPost provides multiple posts.

If a multi-item carousel is implemented, use a valid Blogger data source such as PopularPosts or another compatible architecture.

## Post Cards

Make the following design tokens configurable where practical:

* Card background
* Card title color
* Thumbnail presentation
* Card radius
* Card shadow
* Card spacing
* Typography hierarchy

## Sidebar

The sidebar MUST be a real Blogger section.

It must support movable Blogger widgets such as:

* PopularPosts
* Labels
* Profile
* HTML
* Image
* LinkList

The sidebar must be independently styled and responsive.

## Footer

Use multiple real Blogger sections.

Support:

* Text
* LinkList
* HTML
* Social links
* Footer widgets

---

# Visual Interpretation Rules

Before coding, inspect the reference image and identify:

1. Layout model
2. Header proportions
3. Navigation placement
4. Main content/sidebar ratio
5. Card structure
6. Image proportions
7. Typography hierarchy
8. Spacing system
9. Border radius language
10. Shadow/elevation style
11. Color relationships
12. Mobile behavior implied by the design

Then create a **new visual system** inspired by those principles.

Do not reproduce the exact pixel values unless they are necessary for standard responsive behavior.

The final theme should look like it belongs to the same design family, but it must clearly be an original implementation.

---

# Design Priority

If the reference image conflicts with Blogger functionality, prioritize:

1. Blogger compatibility
2. XML validity
3. Native Blogger functionality
4. SEO
5. Accessibility
6. Performance
7. Responsive design
8. Visual similarity

Never sacrifice Blogger compatibility merely to reproduce a visual detail.

---

# CSS Architecture

Use a centralized design-token architecture.

Example:

`:root`

should consume Blogger Theme Designer variables where available:

`--primary-color: $(primaryColor);`

`--background-color: $(bodyBackground);`

`--surface-color: $(surfaceColor);`

`--text-color: $(textColor);`

`--muted-color: $(mutedTextColor);`

`--border-color: $(borderColor);`

All components should consume these tokens rather than repeating hard-coded colors throughout the stylesheet.

Avoid unnecessary hard-coded visual values.

---

# Dark Mode

Implement a persistent dark mode.

Requirements:

* Preserve the user's selected primary/brand colors.
* Do not replace `$(primaryColor)` with a completely different hard-coded color.
* Invert surface/background/text tokens intelligently.
* Maintain WCAG-compatible contrast.
* Persist preference using `localStorage`.
* Respect `prefers-color-scheme` where appropriate.

---

# Responsive Design

The reference image is only a desktop visual reference unless it clearly contains mobile information.

Create independent responsive behavior for:

* Desktop
* Tablet
* Mobile

Recommended breakpoints:

* Large desktop
* 1024px
* 768px
* 480px

Do not simply shrink the desktop layout.

Reflow:

Header → mobile navigation

Main + Sidebar → stacked layout

Post grid → fewer columns

Hero → responsive composition

Footer → stacked columns

---

# SEO Requirements

Implement modern Blogger SEO architecture:

* One logical H1 per page where appropriate
* Semantic H2/H3 hierarchy
* Article metadata
* Breadcrumbs
* Canonical Blogger URLs
* Proper image alt handling
* Article structured data where compatible
* Open Graph metadata where appropriate
* Twitter/X card metadata where appropriate
* Avoid duplicate title structures
* Avoid unnecessary hidden content

Do not generate invalid Blogger expressions.

---

# Performance Requirements

Keep the theme lightweight.

Avoid:

* Large JavaScript frameworks
* Unnecessary libraries
* Duplicate CSS
* Excessive DOM nodes
* Blocking scripts
* Unnecessary external requests

Prefer:

* Vanilla JavaScript
* CSS Grid
* Flexbox
* Native browser APIs
* Lazy-loaded images where compatible
* Minimal JavaScript

---

# XML Safety

The final file MUST be valid Blogger XML.

Pay special attention to:

* `&amp;`
* `<`
* `>`
* quotes inside attributes
* CDATA blocks
* closing tags
* Blogger namespace declarations
* valid `<b:section>` structure
* valid `<b:widget>` structure

Never place arbitrary HTML directly inside `<b:section>`.

A `<b:section>` should contain only valid Blogger widgets.

---

# Existing Rules

Before modifying the theme:

1. Read and obey:

`rules/blogger-theme-designer.md`

2. Read and obey:

`rules/blogger-xml-safety.md`

3. Treat both files as mandatory architecture rules.

If any instruction conflicts with Blogger's actual XML requirements, choose the technically valid Blogger implementation and document the deviation.

---

# Implementation Workflow

Do NOT immediately start writing the entire theme.

First:

### STEP 1 — Visual Analysis

Analyze the reference image.

Create a short internal design specification containing:

* Layout
* Components
* Visual hierarchy
* Typography
* Colors
* Spacing
* Responsive behavior

### STEP 2 — Architecture Mapping

Map every visual component to a Blogger component.

Example:

Reference Header
→ Header widget + LinkList

Reference Hero
→ FeaturedPost

Reference Article Grid
→ Blog1

Reference Sidebar
→ Sidebar section + widgets

Reference Footer
→ Multiple Blogger sections

### STEP 3 — Theme Designer Mapping

For every major component ask:

"Should the user be able to change this from Blogger Theme Designer?"

If YES:
create a valid `color` or `font` variable where supported.

If NO:
use a centralized CSS design token.

### STEP 4 — Implementation

Build the theme from scratch.

Do not copy the reference implementation.

### STEP 5 — Validation

Run:

* XML validation
* Blogger syntax validation
* Theme Designer variable audit
* Widget/section validation
* Responsive CSS audit
* SEO audit
* Accessibility audit
* JavaScript audit

### STEP 6 — Final Audit

Produce a report containing:

1. Components implemented
2. Blogger widgets used
3. Theme Designer variables created
4. CSS tokens created
5. Responsive behavior
6. SEO implementation
7. Performance considerations
8. XML validation result
9. Remaining risks

---

# CRITICAL RULE

The reference image defines the **design direction**, not the implementation.

Create:

**Same design language + similar visual hierarchy + original implementation + full Blogger functionality + Theme Designer configurability.**

Do not create:

**Pixel-perfect clone + copied structure + copied CSS + static landing page.**
