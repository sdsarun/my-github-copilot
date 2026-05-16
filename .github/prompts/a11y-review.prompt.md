---
mode: agent
description: Accessibility review — WCAG 2.2 compliance, semantic HTML, keyboard navigation, screen reader compatibility, and color contrast
---

# Accessibility Review

Review the selected code for accessibility compliance. Report findings only.

## Scope

Apply WCAG 2.2 Level AA as the minimum standard.

## Review Priorities

### CRITICAL — Perceivable

- Missing `alt` text on `<img>` — add descriptive alt or `alt=""` for decorative images
- Non-text content (icons, SVGs) without accessible label — use `aria-label` or `aria-labelledby`
- Color is the only means of conveying information — add text label, pattern, or icon
- Text contrast ratio below 4.5:1 (normal text) or 3:1 (large text ≥ 18pt or 14pt bold)
- UI component contrast ratio below 3:1 (focus indicator, form border, checkbox outline)
- Images of text — use real text with CSS styling instead

### CRITICAL — Operable

- Interactive elements not reachable by keyboard (`Tab` navigation)
- Custom interactive components (div-as-button) without `role="button"` and `onKeyDown` handler for Enter/Space
- No visible focus indicator on interactive elements — never use `outline: none` without a custom focus style
- Focus trap in modal without proper `aria-modal` and focus management on open/close
- Touch targets below 44×44 CSS pixels (WCAG 2.5.5)
- No skip navigation link to bypass repeated header/nav content

### CRITICAL — Understandable

- Form inputs missing associated `<label>` (via `for` / `id` or `aria-labelledby`)
- Error messages not associated with their field via `aria-describedby`
- Page language not declared: `<html lang="en">`

### HIGH — Robust

- `aria-*` attributes on elements where they do not apply (e.g., `aria-expanded` on a non-interactive `div`)
- Interactive component role without required child roles (e.g., `role="listbox"` without `role="option"` children)
- Status/alert messages not announced: use `role="alert"` or `aria-live="polite"`
- Dynamic content updates not announced to screen readers

### MEDIUM — Best Practices

- Heading hierarchy skipped (e.g., `h1` followed by `h3`) — use logical nesting
- Links with non-descriptive text: "click here", "read more" — include meaningful text or `aria-label`
- Missing `autocomplete` attributes on common personal data fields

## Output Format

```
**[CRITICAL|HIGH|MEDIUM|LOW]** — [File:Line if known]
WCAG Criterion: [e.g., 1.1.1 Non-text Content]
Issue: [What is wrong]
Fix: [Concrete code or attribute suggestion]
```

End with:

```
## Summary
- Critical: N
- High: N
- Medium: N
- WCAG AA compliant: yes / no / partial
```
