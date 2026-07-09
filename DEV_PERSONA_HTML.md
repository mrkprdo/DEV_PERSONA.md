# DEV_PERSONA_HTML - https://github.com/mrkprdo/DEV_PERSONA.md

HTML 5 development standards and conventions for semantic, accessible, maintainable markup.

## Language Standard

- **HTML 5** — use semantic elements, drop XHTML syntax
- **Encoding**: UTF-8 always (`<meta charset="utf-8">`)
- **Doctype**: `<!DOCTYPE html>` (lowercase, HTML 5 style)

## File Naming & Extensions

| Type           | Extension | Convention       |
| -------------- | --------- | ---------------- |
| Page           | `.html`   | `index.html`, `snake_case.html` |
| Template       | `.html`   | `_snake_case.html` (partial) |
| Component      | `.html`   | component file (in framework context) |
| Config/Meta    | `.json`   | `manifest.json` |

## Indentation & Braces

- **2 spaces** (web standard)
- Self-closing tags: `<br>`, `<hr>`, `<input>` (no trailing slash in HTML 5)
- **Max line length**: 100 characters
- Attributes: alphabetical order when possible

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Page Title</title>
  </head>
  <body>
    <header>
      <nav>Navigation</nav>
    </header>

    <main>
      <section>
        <h1>Heading</h1>
        <p>Paragraph content.</p>
      </section>
    </main>

    <footer>Footer</footer>
  </body>
</html>
```

## Naming Conventions

| Category       | Pattern       | Example                    |
| -------------- | ------------- | -------------------------- |
| ID             | `kebab-case`  | `id="main-content"`, `id="app-root"` |
| Class          | `kebab-case`  | `class="sensor-card"`, `class="button--primary"` |
| Data attribute | `kebab-case`  | `data-sensor-id="42"`, `data-test-id="submit-btn"` |
| File slug      | `kebab-case`  | `about-us.html`, `contact-form.html` |

## Semantic Structure

**Always use semantic elements** — they improve accessibility and SEO:

```html
<!-- Good: semantic -->
<header>
  <nav>...</nav>
</header>

<main>
  <article>
    <h1>Article Title</h1>
    <section>
      <h2>Section Heading</h2>
      <p>Content</p>
    </section>
  </article>

  <aside>
    <h3>Related Links</h3>
    <nav>...</nav>
  </aside>
</main>

<footer>
  <p>&copy; 2026 Company</p>
</footer>

<!-- Avoid: divitis (excessive divs) -->
<div class="header">
  <div class="nav">...</div>
</div>
```

### Semantic Elements by Purpose

| Element        | Purpose                             |
| -------------- | ----------------------------------- |
| `<header>`     | Introductory content, site title, nav |
| `<nav>`        | Navigation links (primary or secondary) |
| `<main>`       | Primary content (ONE per page)      |
| `<article>`    | Self-contained content (blog post, comment) |
| `<section>`    | Thematic grouping with heading      |
| `<aside>`      | Tangential content (sidebar, related) |
| `<footer>`     | Footer, metadata, copyright         |
| `<figure>`     | Self-contained illustration/diagram |
| `<figcaption>` | Caption for figure                  |
| `<details>`    | Collapsible disclosure widget       |
| `<summary>`    | Summary for details (interactive)   |
| `<time>`       | Date/time (with `datetime` attribute) |
| `<address>`    | Author/contact information          |

## Attributes & Best Practices

### Always include

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Charset MUST be first meta tag -->
    <meta charset="utf-8">
    <!-- Viewport for mobile responsiveness -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- Descriptive title -->
    <title>Page Title - Site Name</title>
    <!-- Favicons -->
    <link rel="icon" href="/favicon.ico">
    <link rel="apple-touch-icon" href="/apple-touch-icon.png">
  </head>
</html>
```

### Form best practices

```html
<!-- Always use <label> with for/id binding -->
<form method="post" action="/submit">
  <fieldset>
    <legend>Contact Information</legend>

    <label for="name">Full Name</label>
    <input
      id="name"
      type="text"
      name="full_name"
      required
      autocomplete="name"
    >

    <label for="email">Email</label>
    <input
      id="email"
      type="email"
      name="email"
      required
      autocomplete="email"
    >

    <label for="message">Message</label>
    <textarea id="message" name="message" required></textarea>

    <button type="submit">Send</button>
  </fieldset>
</form>
```

### Images & media

```html
<!-- Always alt text on images -->
<img
  src="/sensor-chart.png"
  alt="Sensor temperature readings over 24 hours"
  width="800"
  height="400"
>

<!-- Responsive images with srcset -->
<img
  src="/image-medium.png"
  srcset="/image-small.png 320w, /image-medium.png 640w, /image-large.png 1280w"
  sizes="(max-width: 640px) 100vw, 50vw"
  alt="Descriptive alt text"
>

<!-- Video with fallback -->
<video width="640" height="480" controls>
  <source src="/video.mp4" type="video/mp4">
  Your browser does not support HTML5 video.
</video>
```

### Lists

```html
<!-- Ordered (sequence matters) -->
<ol>
  <li>First step</li>
  <li>Second step</li>
  <li>Third step</li>
</ol>

<!-- Unordered (sequence doesn't matter) -->
<ul>
  <li>Item A</li>
  <li>Item B</li>
  <li>Item C</li>
</ul>

<!-- Description list -->
<dl>
  <dt>Temperature</dt>
  <dd>Measured in Celsius</dd>
  <dt>Humidity</dt>
  <dd>Relative humidity percentage</dd>
</dl>
```

## Accessibility (a11y)

**WCAG 2.1 AA minimum**:

1. **Headings**: `<h1>` > `<h2>` > `<h3>` (never skip levels)
2. **Alt text**: every image must have meaningful `alt` (can be empty for decorative: `alt=""`)
3. **Color contrast**: 4.5:1 for text, 3:1 for UI components
4. **Focus visible**: all interactive elements must show focus indicator
5. **ARIA**: use sparingly; prefer semantic HTML. When needed:
   ```html
   <button aria-label="Close dialog" aria-pressed="false">×</button>
   <div role="alert" aria-live="polite">Error message</div>
   ```
6. **Skip links**: hide but keyboard-accessible
   ```html
   <a href="#main" class="skip-link">Skip to main content</a>
   ```

## Comments

**Section dividers only** — for major layout sections:

```html
<!-- ================================================================
     Header & Navigation
     ================================================================ -->
<header>
  ...
</header>

<!-- ================================================================
     Main Content
     ================================================================ -->
<main>
  ...
</main>
```

**Inline comments**: only for non-obvious markup. Explain why, not what.

```html
<!-- Show notification only if user has unsaved changes -->
<div id="unsaved-warning" hidden>
  You have unsaved changes.
</div>
```

## Modern HTML Features — Prefer These

- **`<picture>` element**: for art-directed responsive images
- **`<template>` element**: for client-side rendering blocks
- **`<dialog>` element**: for modals (no jQuery modal plugins)
- **`<details>` / `<summary>`**: for expandable content
- **`<meter>` / `<progress>`**: for gauges and progress bars
- **Web Components**: `<custom-element>` (when using a framework)
- **Microdata**: `itemscope`, `itemtype`, `itemprop` for rich snippets
- **`datetime` attribute**: `<time datetime="2026-07-08">July 8, 2026</time>`
- **Form validation**: HTML5 attributes (`required`, `type="email"`, `pattern=`)
- **`loading="lazy"`**: for off-screen images

## Prohibited

- **Deprecated tags**: `<b>` / `<i>` (use `<strong>` / `<em>`), `<u>` (use CSS), `<font>` (use CSS)
- **Layout with tables**: `<table>` only for tabular data
- **Inline styles**: `style="..."` (use CSS classes)
- **Event handlers in HTML**: `onclick="..."` (use event listeners in JavaScript)
- **`<div>` for everything**: use semantic elements
- **Multiple `<h1>` tags per page**: only ONE logical main heading
- **Images in text**: use CSS `background-image` or `<img>` with semantic context
- **Spacer GIFs** or empty elements for layout

## Comment structure

```html
<!-- Use comment dividers for major sections -->

<!-- ================================================================
     Hero Section
     ================================================================ -->

<!-- Only comment non-obvious markup -->
<article>
  <h1>Article Title</h1>
  <!-- Show publication date in time element for semantic meaning -->
  <time datetime="2026-07-08">July 8, 2026</time>
  <p>Content...</p>
</article>
```

## File Header Template

New HTML files must include this banner in a comment at the top:

```html
<!--
================================================================================
Author: Mark Anthony Prado
Created: [YYYY-MM-DD]
================================================================================
-->

<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- rest of page... -->
```

Example (contact form page):

```html
<!--
================================================================================
Author: Mark Anthony Prado
Created: 2026-07-08
================================================================================
-->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Contact Us - Company Name</title>
    <link rel="icon" href="/favicon.ico">
  </head>
  <body>
    <!-- rest of page... -->
  </body>
</html>
```

Partial component (header navigation):

```html
<!--
================================================================================
Author: Mark Anthony Prado
Created: 2026-07-08
================================================================================
-->

<header>
  <nav>
    <!-- navigation content... -->
  </nav>
</header>
```

## Testing & Validation

- **W3C HTML Validator**: ensure valid markup
- **Accessibility**: axe, Lighthouse, or WAVE browser extensions
- **Responsive design**: test at 320px, 768px, 1024px+ breakpoints
- **Link checker**: all internal/external links valid
