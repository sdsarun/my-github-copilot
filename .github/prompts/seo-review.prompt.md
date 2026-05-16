---
agent: code-reviewer
description: SEO review — crawlability, indexability, Core Web Vitals, structured data, and meta tag coverage
---

# SEO Review

Audit the selected pages or components for SEO correctness. Report findings only.

## Scope

Focus on technical SEO — not content strategy.

## Review Priorities

### CRITICAL — Crawl & Index

- `noindex` meta tag on pages that should be indexed
- `Disallow: /` in `robots.txt` blocking the entire site (common staging misconfiguration)
- `robots.txt` missing entirely — search engines may crawl everything
- Pages critical to discovery not linked from any other indexed page (orphan pages)

### CRITICAL — URL & Canonicalization

- Missing canonical `<link rel="canonical">` on all indexable pages — duplicate content risk
- Canonical pointing to a different URL than the page actually at — misconfigured
- `www` and non-`www` versions both resolving without a redirect — duplicate content
- HTTP not redirecting to HTTPS (301)

### HIGH — Meta & Titles

- Missing `<title>` tag — search engines will generate one from page content
- Duplicate `<title>` across multiple pages — differentiate per page
- Missing `<meta name="description">` — not a ranking factor but affects CTR
- `<title>` over 60 characters — may be truncated in results
- `<meta name="description">` over 160 characters

### HIGH — Core Web Vitals (Performance as Ranking Signal)

- LCP (Largest Contentful Paint) — image or heading not preloaded: add `<link rel="preload">`
- CLS (Cumulative Layout Shift) — images missing `width` and `height` attributes — browser can't reserve space
- INP / FID — heavy JavaScript blocking main thread at page load

### HIGH — Structured Data

- Missing `application/ld+json` JSON-LD blocks for content types that benefit: Article, Product, FAQ, BreadcrumbList, Organization
- Structured data present but using deprecated or invalid properties — validate with Google Rich Results Test

### MEDIUM — Open Graph & Social

- Missing `og:title`, `og:description`, `og:image` on shareable pages
- `og:image` smaller than 1200×630 pixels — low quality preview on social platforms
- Missing `twitter:card` meta tag

### MEDIUM — Sitemap

- No XML sitemap linked from `robots.txt`
- Sitemap includes noindex'd or redirect URLs

## Output Format

```
**[CRITICAL|HIGH|MEDIUM|LOW]** — [File:Line / URL if known]
Issue: [What is wrong]
Fix: [Concrete HTML, config, or code change]
```

End with:

```
## Summary
- Critical: N
- High: N
- Medium: N
- SEO-ready: yes / no / partial
```
