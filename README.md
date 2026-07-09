# DEV_PERSONA.md

Curated developer personas for multiple languages. Each persona defines coding standards, naming conventions, structure, and best practices tailored to the language.

## Table of Contents

- [C++](#cpp)
- [Python](#python)
- [TypeScript](#typescript)
- [CSS](#css)
- [HTML](#html)

---

## C++

**File**: [DEV_PERSONA_CPP.md](DEV_PERSONA_CPP.md)

C++23 standards with Allman bracing, Hungarian-style prefixes (lVariable, mMember, sStatic), strict Doxygen documentation in headers, and zero comments in source files. Emphasis on modern C++23 features (`std::expected`, `std::optional`, `std::span`).

**Key points**:
- Standard: C++23
- Indent: 4 spaces, Allman style
- Naming: `lPascalCase` (local), `mPascalCase` (member), `UPPER_SNAKE` (const)
- Headers: Full Doxygen docs; source: zero comments
- Features: `constexpr`, `std::optional`, `std::expected`, `[[nodiscard]]`

---

## Python

**File**: [DEV_PERSONA_PY.md](DEV_PERSONA_PY.md)

Python 3.11+ with strict type hints, Google-style docstrings, and snake_case naming. Emphasis on context managers, dataclasses, and logging over print.

**Key points**:
- Standard: Python 3.11+
- Indent: 4 spaces, PEP 8
- Naming: `snake_case` (functions), `PascalCase` (classes), `UPPER_SNAKE` (constants)
- Docstrings: Google style on all public exports
- Features: type hints, `dataclass`, `Enum`, context managers, `logging` module

---

## TypeScript

**File**: [DEV_PERSONA_TS.md](DEV_PERSONA_TS.md)

TypeScript 5.0+ with strict mode, camelCase naming, and TSDoc comments. Prefer discriminated unions, async/await, and type inference where obvious.

**Key points**:
- Standard: TypeScript 5.0+, `strict: true`
- Indent: 2 spaces
- Naming: `camelCase` (functions), `PascalCase` (classes/types), `UPPER_SNAKE` (constants)
- Comments: TSDoc/JSDoc on all public exports
- Features: type inference, union types, `as const`, discriminated unions, async/await

---

## CSS

**File**: [DEV_PERSONA_CSS.md](DEV_PERSONA_CSS.md)

Modern CSS 3/4 with utility-first approach (Tailwind) or vanilla CSS with BEM naming. CSS variables for theming, Grid/Flexbox for layout, mobile-first responsive design.

**Key points**:
- Standard: CSS 3+
- Indent: 2 spaces
- Naming: `kebab-case` (classes), `--kebab-case` (variables), BEM for components
- Structure: section comments, CSS variables, mobile-first breakpoints
- Features: Grid, Flexbox, `clamp()`, container queries, logical properties, theme support

---

## HTML

**File**: [DEV_PERSONA_HTML.md](DEV_PERSONA_HTML.md)

HTML 5 with semantic elements, accessibility-first (WCAG 2.1 AA), and proper form/media handling. Never divitis, always `<label>` with forms, always alt text on images.

**Key points**:
- Standard: HTML 5
- Indent: 2 spaces
- Naming: `kebab-case` (IDs/classes), `data-kebab-case` (attributes)
- Semantics: use `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, `<footer>`
- Accessibility: WCAG 2.1 AA, headings hierarchy, skip links, ARIA sparingly
- Features: `<dialog>`, `<details>`, `<picture>`, `<time>`, form validation

---

## Usage

Each persona file is self-contained and can be referenced independently or incorporated into project-specific `.claude/CLAUDE.md` files. Use them to:

- Enforce consistent coding standards across teams
- Onboard new developers to language-specific conventions
- Review code against established guidelines
- Generate project-specific personas by extending or customizing these templates

---

**Created**: July 8, 2026  
**Curator**: Mark Anthony Prado
