# BLOGGER THEME DESIGNER — TEKMAX MASTER RULE

## STATUS

This is a mandatory engineering rule for the Blogger theme project.

The AI Agent MUST follow this rule whenever creating, modifying, refactoring, or reviewing the Blogger XML theme.

---

# 1. CORE PRINCIPLE

Every major visual component, section, widget, and user-facing UI element MUST expose its appropriate customization options through the native Blogger Theme Designer whenever technically supported.

The theme MUST be designed as a configurable Blogger theme, NOT as a hard-coded HTML/CSS template.

Before implementing a component, determine which properties should be user-configurable.

---

# 2. CONFIGURATION-FIRST DEVELOPMENT

NEVER immediately code a new component.

For every new component:

1. Identify the component.
2. Identify configurable properties.
3. Define Blogger variables/settings.
4. Connect those variables to CSS/HTML/JS.
5. Implement the component.
6. Test the configuration.
7. Verify responsive behavior.
8. Verify Blogger XML validity.

A component is NOT complete until its configuration has been implemented and verified.

---

# 3. MANDATORY COMPONENT CONFIGURATION

The following components MUST have appropriate Theme Designer configuration.

## LOGO

Configurable where applicable:

* Logo image
* Logo width
* Logo height
* Logo spacing
* Logo alignment
* Logo border radius
* Logo visibility
* Desktop size
* Mobile size

Example variable naming:

logoWidth
logoHeight
logoSpacing
logoRadius
logoVisible

---

# HEADER

Configurable:

* Header background
* Header height
* Header padding
* Header border
* Header shadow
* Header radius where applicable
* Logo position
* Navigation visibility
* Search visibility
* Dark mode visibility
* Mobile menu visibility
* Sticky header
* Desktop layout
* Mobile layout

Example:

headerBackground
headerHeight
headerPadding
headerBorder
headerShadow
headerSticky

---

# NAVIGATION

Configurable:

* Navigation font size
* Font weight
* Text color
* Hover color
* Active color
* Item spacing
* Horizontal padding
* Border radius
* Dropdown behavior
* Mobile navigation visibility
* Mobile menu behavior

Example:

navFontSize
navFontWeight
navTextColor
navHoverColor
navActiveColor
navItemGap

---

# SIDEBAR

Configurable:

* Sidebar width
* Sidebar gap
* Background
* Padding
* Border
* Border color
* Border radius
* Shadow
* Sticky behavior
* Sticky offset
* Widget spacing
* Widget title size
* Widget title color
* Mobile visibility

Example:

sidebarWidth
sidebarGap
sidebarBackground
sidebarPadding
sidebarBorder
sidebarRadius
sidebarShadow
sidebarSticky
sidebarStickyOffset

---

# SLIDER / HERO

Configurable:

* Height
* Width
* Border radius
* Autoplay
* Autoplay interval
* Animation type
* Animation duration
* Arrows
* Dots
* Arrow size
* Overlay
* Overlay opacity
* Title size
* Description size
* Button visibility
* Button styling
* Mobile height

Example:

sliderHeight
sliderRadius
sliderAutoplay
sliderInterval
sliderAnimation
sliderAnimationDuration
sliderArrows
sliderDots
sliderOverlay
sliderOverlayOpacity

---

# POST CARDS

Configurable:

* Card background
* Card padding
* Card gap
* Image ratio
* Image height
* Image radius
* Card radius
* Card shadow
* Title size
* Title color
* Meta size
* Meta color
* Excerpt size
* Hover effect

Example:

postCardBackground
postCardPadding
postCardGap
postCardRadius
postCardShadow
postCardTitleSize
postCardTitleColor

---

# SINGLE POST

Configurable:

* Article width
* Title size
* Content font size
* Line height
* Paragraph spacing
* Heading sizes
* Image radius
* Blockquote style
* Code block appearance
* Table appearance
* Reading time visibility
* Reading progress visibility
* Table of contents visibility
* Share buttons visibility

Example:

postContentWidth
postTitleSize
postContentSize
postLineHeight
postParagraphSpacing
postImageRadius

---

# FOOTER

Configurable:

* Background
* Text color
* Link color
* Padding
* Widget spacing
* Columns
* Border
* Social icons visibility
* Copyright visibility
* Typography

Example:

footerBackground
footerTextColor
footerLinkColor
footerPadding
footerWidgetGap

---

# 4. GLOBAL THEME VARIABLES

Create a centralized global configuration system.

Global configuration SHOULD include:

## COLORS

primaryColor
secondaryColor
accentColor
bodyBackground
surfaceColor
textColor
mutedTextColor
borderColor
linkColor

## TYPOGRAPHY

bodyFont
headingFont
bodyFontSize
bodyLineHeight
headingFontWeight

## LAYOUT

containerWidth
contentWidth
sidebarWidth
mainGap

## DESIGN

globalRadius
globalShadow
globalSpacing

Components SHOULD reuse global variables where appropriate.

Do not create duplicate variables when a suitable global variable already exists.

---

# 5. RESPONSIVE CONFIGURATION

Where meaningful, support separate values or responsive behavior for:

* Desktop
* Tablet
* Mobile

At minimum, mobile behavior MUST be considered for:

* Header
* Logo
* Navigation
* Sidebar
* Slider
* Post cards
* Typography
* Spacing

Do NOT blindly create separate variables for every property.

Only create separate responsive configuration when it provides meaningful user control.

---

# 6. NO UNNECESSARY HARDCODING

The following types of values SHOULD NOT be hard-coded when they represent user-facing design choices:

* Colors
* Major dimensions
* Typography
* Spacing
* Border radius
* Shadows
* Visibility
* Animation settings
* Layout widths

Bad:

```css
.sidebar {
    width: 330px;
    padding: 20px;
    border-radius: 12px;
}
```

Preferred:

```css
.sidebar {
    width: $(sidebarWidth);
    padding: $(sidebarPadding);
    border-radius: $(sidebarRadius);
}
```

---

# 7. EXCEPTIONS TO THE NO-HARDCODING RULE

Hard-coded values ARE allowed when they are:

1. Structural constants.
2. Required by browser behavior.
3. Required by Blogger.
4. Required for accessibility.
5. Required for CSS/JS functionality.
6. A technical fallback.
7. A zero value with no meaningful user customization.
8. A value that would create unnecessary configuration complexity.

Do NOT create a Theme Designer variable merely for the sake of creating one.

Configuration must provide real user value.

---

# 8. JAVASCRIPT CONFIGURATION

JavaScript behavior that users reasonably expect to configure MUST NOT be permanently hard-coded.

Examples:

BAD:

```javascript
autoplay = true;
interval = 5000;
```

Preferred architecture:

```text
Blogger configuration
        ↓
Template variable/settings
        ↓
HTML/data attribute
        ↓
JavaScript
```

Example concept:

```html
data-autoplay="$(sliderAutoplay)"
data-interval="$(sliderInterval)"
```

JavaScript then reads the configuration.

---

# 9. WIDGET CONFIGURATION

Every major Blogger widget MUST have an appropriate configuration strategy.

When Blogger provides native widget settings, use them.

When native Theme Designer variables are more appropriate, use `<Variable>` definitions.

When neither mechanism supports a desired option, DO NOT invent unsupported Blogger syntax.

Instead:

1. Identify the limitation.
2. Use the safest Blogger-compatible solution.
3. Document the limitation.
4. Continue without breaking Blogger compatibility.

---

# 10. VARIABLE NAMING STANDARD

Use predictable camelCase names.

Correct:

```text
logoWidth
logoHeight
headerHeight
sidebarWidth
sidebarGap
sliderHeight
sliderAutoplay
sliderInterval
postCardRadius
footerBackground
```

Avoid:

```text
Logo_W
lgwidth
sideW
sliderThing
custom1
settingX
```

Variable names MUST clearly describe their purpose.

---

# 11. VARIABLE ORGANIZATION

Organize variables logically.

Recommended order:

```text
GLOBAL
├── Colors
├── Typography
├── Layout
└── Design

HEADER
├── Logo
├── Header
└── Navigation

CONTENT
├── Slider
├── Post Cards
└── Single Post

SIDEBAR
├── Sidebar
└── Sidebar Widgets

FOOTER
└── Footer
```

---

# 12. CONFIGURATION MATRIX

Before implementing the complete theme, maintain an internal configuration matrix.

Format:

```text
Component | Setting | Type | Default | Variable | Responsive | Consumer
```

Example:

```text
Logo | Width | length | 180px | logoWidth | Desktop/Mobile | CSS
Sidebar | Width | length | 330px | sidebarWidth | Desktop | CSS
Slider | Autoplay | boolean | true | sliderAutoplay | All | JS
Slider | Interval | number | 5000 | sliderInterval | All | JS
Post Card | Radius | length | 12px | postCardRadius | All | CSS
```

The matrix MUST be checked before declaring the theme complete.

---

# 13. CONFIGURATION COVERAGE AUDIT

When reviewing the theme, inspect every major component.

Ask:

```text
Does this component have user-configurable visual properties?

If YES:
Are they exposed through Blogger configuration?

If YES:
Are they actually connected to CSS/HTML/JS?

If YES:
Does changing the setting produce the expected result?

If NO:
Is the property intentionally structural?

If not:
Create the required configuration.
```

---

# 14. QUALITY GATE

Before declaring the Blogger theme complete:

```text
[ ] Logo configurable
[ ] Header configurable
[ ] Navigation configurable
[ ] Sidebar configurable
[ ] Slider configurable
[ ] Post cards configurable
[ ] Single post configurable
[ ] Footer configurable
[ ] Global colors configurable
[ ] Typography configurable
[ ] Major spacing configurable
[ ] Major layout dimensions configurable
[ ] Responsive behavior verified
[ ] JavaScript configurable behavior connected correctly
[ ] No unnecessary hard-coded design values
[ ] Variable naming is consistent
[ ] No duplicate configuration variables
[ ] Blogger XML remains valid
[ ] Blogger-specific syntax is valid
[ ] Theme Designer variables work correctly
[ ] Configuration changes have been tested
```

---

# 15. IMPORTANT DEVELOPMENT BEHAVIOR

When the user asks:

"Add a new widget"

DO NOT simply add HTML/CSS.

Instead automatically perform:

```text
1. Analyze widget.
2. Define configuration.
3. Define variables.
4. Implement widget.
5. Connect variables.
6. Add responsive behavior.
7. Validate Blogger XML.
8. Verify configuration.
```

When the user asks:

"Change the design"

First determine whether the requested property should be a configurable Theme Designer variable.

If yes, modify the variable architecture rather than introducing another hard-coded value.

---

# 16. FINAL RULE

The objective is NOT to maximize the number of variables.

The objective is to create a professional Blogger Theme Designer experience where a user can customize the theme without editing XML whenever Blogger technically allows it.

Configuration must be:

* Native where possible
* Predictable
* Centralized
* Maintainable
* Responsive
* Blogger-compatible
* Performance-conscious
* Free from unnecessary duplication

NEVER sacrifice Blogger XML validity or runtime stability merely to make a property configurable.

When there is a conflict:

```text
Blogger Compatibility
        >
Runtime Stability
        >
Maintainability
        >
Configuration Flexibility
        >
Cosmetic Convenience
```
