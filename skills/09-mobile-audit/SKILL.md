---
name: mobile-audit
description: Use when auditing mobile responsiveness, after any major UI change, or when the user reports mobile layout issues. Tests systematically at 375px viewport width (iPhone SE — the smallest common screen).
---

# Mobile Audit — 375px Systematic Check

Test at 375px viewport width. Return PASS ✅ or FAIL ❌ for each item.

## Layout
- [ ] Zero horizontal overflow (no sideways scroll at any point in the app)
- [ ] All text readable at default zoom (no overflow, no truncation without ellipsis)
- [ ] Images contained within their parent elements
- [ ] Data tables have a mobile layout (cards, or horizontal scroll with sticky first column)

## Navigation
- [ ] Sidebar is hidden on mobile (replaced by bottom nav or hamburger)
- [ ] Bottom nav has max 5 items with labels
- [ ] Active state clearly visible on bottom nav
- [ ] Hamburger/drawer closes when tapping outside or swiping left

## Touch Targets
- [ ] All buttons and links ≥ 44×44px tappable area
- [ ] No overlapping touch targets
- [ ] Sufficient spacing between adjacent list items (min 8px)
- [ ] Floating action buttons not obscured by bottom nav

## Forms & Inputs
- [ ] `type="email"` on email fields (shows correct keyboard)
- [ ] `type="tel"` on phone fields
- [ ] `type="number"` with `inputmode="decimal"` on numeric fields
- [ ] No input fields hidden behind the mobile keyboard when focused
- [ ] Textareas auto-resize instead of requiring manual drag

## Modals & Overlays
- [ ] All modals render as bottom sheets on mobile
- [ ] Bottom sheets have a visible drag handle
- [ ] Bottom sheets dismissible with swipe-down gesture
- [ ] No modal or sheet taller than 90vh without internal scrolling

## Floorplans, Maps & Zoomable Content
- [ ] Pinch-zoom enabled
- [ ] Pan works without fighting page scroll
- [ ] Zoom level resets on close/reopen

## PWA (if enabled)
- [ ] Install banner displayed on first mobile visit
- [ ] App icon and name correct on home screen
- [ ] Offline fallback page exists

## End-to-End Flow Test
Walk the primary user journey at 375px:
1. Landing / home → 2. Sign up or log in → 3. Core feature → 4. Settings → 5. Sign out
Report any step that breaks, overflows, or feels wrong on mobile.