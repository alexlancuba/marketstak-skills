---
name: performance-audit
description: Use when optimizing app speed, before a major launch, when Lighthouse scores are below 90, or when users report the app feeling slow. Targets 90+ across all four Lighthouse categories.
---

# Performance & Lighthouse Audit

Target: 90+ on Performance, Accessibility, Best Practices, and SEO.

## Images
- [ ] All images use WebP or AVIF format
- [ ] `width` and `height` attributes set on all `<img>` tags (prevents CLS)
- [ ] Lazy loading on all images below the fold (`loading="lazy"`)
- [ ] Hero / above-fold images preloaded (`<link rel="preload">`)
- [ ] No image served larger than its display size

## JavaScript Bundle
- [ ] Route-level code splitting with lazy imports for heavy pages
- [ ] No large libraries imported globally if only used on one page
- [ ] Heavy components (charts, rich text editors, maps) lazy-loaded
- [ ] No unused exports imported in critical path

## Rendering & CLS
- [ ] Skeleton loaders on all async data (no layout shift when data loads)
- [ ] No content jumping after initial render
- [ ] Fonts loaded with `font-display: swap`

## Network & Data
- [ ] API responses cached with SWR or React Query where appropriate
- [ ] No redundant API calls on component mount
- [ ] Supabase queries select only needed columns — no `SELECT *` on large tables
- [ ] Pagination or virtual scrolling on lists over 50 items

## Cleanup
- [ ] All `console.log` removed from production code and edge functions
- [ ] No unused dependencies in package.json
- [ ] No commented-out code blocks left in production files

## Accessibility (for 90+ score)
- [ ] All images have descriptive `alt` text (not "image" or filename)
- [ ] Color contrast ratio ≥ 4.5:1 for normal text
- [ ] All interactive elements reachable by keyboard (Tab key)
- [ ] ARIA labels on all icon-only buttons

## Report
List each failing item with the specific file/component and the fix.
Estimate the Lighthouse score impact for the top 3 fixes.