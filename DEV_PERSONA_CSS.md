# DEV_PERSONA_CSS - https://github.com/mrkprdo/DEV_PERSONA.md

CSS 3+ development standards and conventions for consistent, maintainable stylesheets.

## Language Standard

- **CSS 3 / CSS 4** — modern baseline (no IE11 support)
- **Preprocessor** (optional): SCSS/SASS when using utility generation or complex nesting
- **Approach**: Utility-first (Tailwind) or CSS-in-JS preferred; vanilla CSS acceptable if well-organized

## File Naming & Extensions

| Type             | Extension | Convention       |
| ---------------- | --------- | ---------------- |
| Stylesheet       | `.css`    | `snake_case.css` |
| SCSS/Sass        | `.scss`   | `snake_case.scss` |
| Component styles | `.module.css` | `PascalComponent.module.css` |
| Config           | `.js`     | `tailwind.config.js` |

## Indentation & Braces

- **2 spaces** (consistent with web standards)
- Brace on same line (K&R style)
- **Max line length**: 100 characters
- One selector per line (multi-selector formatting)

```css
/* Multi-selector: one per line */
.button,
.button-primary,
.button-secondary {
  padding: 0.5rem 1rem;
  border-radius: 4px;
  font-weight: 600;
}

/* Nested (SCSS) */
.card {
  border: 1px solid #ddd;
  padding: 1rem;

  &:hover {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  &__header {
    font-size: 1.25rem;
  }
}
```

## Naming Conventions

| Category          | Pattern       | Example                    |
| ----------------- | ------------- | -------------------------- |
| Class             | `kebab-case`  | `.sensor-card`, `.primary-button` |
| ID                | `kebab-case`  | `#main-content`, `#app-root` |
| CSS variable      | `--kebab-case` | `--color-primary`, `--spacing-unit` |
| SCSS mixin        | `snake_case`  | `@mixin flex_center()` |
| SCSS function     | `snake_case`  | `@function calc_spacing()` |
| BEM modifier      | `--kebab-case` | `.button--primary`, `.card--expanded` |
| State class       | `.is-*`/`.has-*` | `.is-active`, `.has-error` |

### Naming rules

1. Classes: describe the component/element, not the style (``.sensor-card` not `.blue-box`)
2. BEM (Block Element Modifier): `.block__element--modifier` for complex components
3. State classes: prefix with `.is-` (active/disabled/open) or `.has-` (has-content/has-error)
4. Utility classes (if Tailwind not used): short, single-responsibility (`.text-center`, `.flex`, `.mt-2`)

## Comments

**Grouped by section** — describe the purpose, not the selector:

```css
/* ============================================================================
 * Header & Navigation
 * ============================================================================ */

.header {
  /* ... */
}

.nav {
  /* ... */
}

/* ---- Card component ---- */
.card {
  /* ... */
}

.card__header {
  /* ... */
}
```

**JSDoc style for SCSS functions/mixins**:

```scss
/**
 * Create a flex container with centered content.
 *
 * @param {number} $gap - Gap between items (default: 1rem).
 */
@mixin flex_center($gap: 1rem) {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: $gap;
}
```

## Structure & Organization

### Vanilla CSS (BEM + Section Comments)

```css
/* ============================================================================
 * Colors & Variables
 * ============================================================================ */

:root {
  --color-primary: #3498db;
  --color-success: #27ae60;
  --color-error: #e74c3c;
  --spacing-unit: 1rem;
  --border-radius: 4px;
  --transition-default: all 0.2s ease;
}

/* ============================================================================
 * Reset / Utilities
 * ============================================================================ */

*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  scroll-behavior: smooth;
}

/* ============================================================================
 * Components
 * ============================================================================ */

/* ---- Button ---- */
.button {
  padding: var(--spacing-unit) calc(var(--spacing-unit) * 2);
  border-radius: var(--border-radius);
  cursor: pointer;
  transition: var(--transition-default);
}

.button--primary {
  background-color: var(--color-primary);
  color: white;
}

.button:hover {
  opacity: 0.9;
}

.button:disabled,
.button.is-disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* ---- Card ---- */
.card {
  border: 1px solid #ddd;
  border-radius: var(--border-radius);
  padding: var(--spacing-unit);
}

.card__header {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.card__body {
  font-size: 0.95rem;
  color: #666;
}
```

### SCSS with Utilities

```scss
// ============================================================================
// Variables & Maps
// ============================================================================

$colors: (
  "primary": #3498db,
  "success": #27ae60,
  "error": #e74c3c,
);

$spacing: (
  "xs": 0.25rem,
  "sm": 0.5rem,
  "md": 1rem,
  "lg": 1.5rem,
  "xl": 2rem,
);

// ============================================================================
// Mixins
// ============================================================================

@mixin flex_center($gap: 1rem) {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: $gap;
}

@mixin respond_to($breakpoint) {
  @if $breakpoint == "mobile" {
    @media (max-width: 640px) {
      @content;
    }
  } @else if $breakpoint == "tablet" {
    @media (max-width: 1024px) {
      @content;
    }
  }
}

// ============================================================================
// Components
// ============================================================================

.button {
  padding: map-get($spacing, "md") map-get($spacing, "lg");
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s ease;

  &--primary {
    background-color: map-get($colors, "primary");
    color: white;

    &:hover {
      opacity: 0.9;
    }
  }

  &:disabled,
  &.is-disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
}
```

## Modern CSS Features — Prefer These

- **CSS Variables** (`--name`) for theming and dynamic values
- **Grid** (`display: grid`) for layouts; Flexbox for components
- **Logical properties**: `margin-block`, `padding-inline` (i18n-friendly)
- **Container queries**: `@container` for component-scoped media queries (modern browsers)
- **Cascade layers** (`@layer`): organize specificity deliberately
- **`aspect-ratio`**: for maintaining proportions
- **CSS functions**: `clamp()`, `min()`, `max()` for responsive values
- **Subgrid**: for nested grid alignment
- **`:is()`, `:where()`, `:has()`**: for flexible selectors without bloat

## Prohibited

- `!important` (except overrides in legacy code — refactor out)
- Inline styles in HTML (use classes)
- IDs for styling (use classes — only 1 ID per page for page anchors)
- Magic numbers without context (use variables/functions)
- Deprecated properties: `float` for layout (Grid/Flex instead)
- Excessive nesting in SCSS (max 3 levels)
- Vendor prefixes (let autoprefixer handle them)
- Calc with non-matching units: `calc(1rem + 5px)` is OK; `calc(1 + 2rem)` is not

## Responsive Design

- **Mobile-first**: base styles are mobile, use `@media (min-width: ...)` for larger
- **Common breakpoints**:
  ```scss
  $breakpoints: (
    "mobile": 0,
    "tablet": 640px,
    "desktop": 1024px,
    "wide": 1280px,
  );
  ```
- **Avoid device-specific breakpoints** — design for content reflow

## Theme Support (Light/Dark)

```css
:root {
  --bg-primary: #ffffff;
  --text-primary: #1a1a1a;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg-primary: #1a1a1a;
    --text-primary: #ffffff;
  }
}

body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
}
```

## File Header Template

New CSS/SCSS files must include this banner at the top:

```css
/**
 * ============================================================================
 * Author: Mark Anthony Prado
 * Created: [YYYY-MM-DD]
 * ============================================================================
 */
```

Example (`button.css`):

```css
/**
 * ============================================================================
 * Author: Mark Anthony Prado
 * Created: 2026-07-08
 * ============================================================================
 */

:root {
  --button-padding: 0.5rem 1rem;
  --button-radius: 4px;
}

.button {
  /* styles... */
}
```

SCSS example (`card.scss`):

```scss
/**
 * ============================================================================
 * Author: Mark Anthony Prado
 * Created: 2026-07-08
 * ============================================================================
 */

$card-padding: 1rem;
$card-border: 1px solid #ddd;

.card {
  // styles...
}
```

## Testing

- Visual regression: `Percy` or `Playwright` screenshots
- Accessibility: `axe` in automated tests
- Performance: Lighthouse CI
