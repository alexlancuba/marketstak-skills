---
name: holistic-edge-design
description: Use when building, redesigning, or updating UI components, pages, or layouts for any Holistic Edge workspace project. Apply whenever a new page, component, or visual pass is requested. Enforces the Apple-inspired glassmorphic design system with teal as the primary brand color.
---

# Holistic Edge Design System

## Brand Identity
- Primary color: Teal `#09acb2`
- Style: Apple-inspired Glassmorphism — clean, intuitive, distraction-free
- Font: System font stack (SF Pro on macOS/iOS, Inter as fallback)
- Personality: Premium, minimal chrome, every element earns its place

## Glassmorphism Rules
- Cards: `bg-white/5 backdrop-blur-md border border-white/10 rounded-2xl`
- Modals and panels: `bg-white/8 backdrop-blur-xl border border-white/10`
- Sidebar: `bg-black/40 backdrop-blur-xl`
- Never use solid opaque backgrounds on cards, drawers, or panels
- Always use `backdrop-blur` — it is the defining visual effect

## Color Palette
- Primary action / brand: `#09acb2` (teal)
- Text primary: `white`
- Text secondary: `white/60`
- Borders: `white/10`
- Hover overlay: `white/5`
- Destructive: `red-500`
- Success: `emerald-500`
- Warning: `amber-500`

## Layout
- Sidebar: 72px collapsed (icons only), 240px expanded, no labels when collapsed
- Top header: sticky, glassmorphic, 64px height
- Content area: max-width 1200px, centered, 24px padding on desktop
- Mobile: bottom navigation bar (max 5 items), sidebar hidden

## Component Standards
- Primary CTAs: `rounded-full`, teal background
- Secondary buttons: `rounded-lg`, `border border-white/10`, transparent background
- Inputs: glassmorphic background, `border-white/10`, `rounded-lg`
- Textareas: auto-resize (no fixed height, grows with content)
- Status chips/badges: `rounded-full`, small, color-coded by status
- Progress indicators: teal fill on subtle track

## Mobile Rules
- All modals become bottom sheets on screens < 768px
- Bottom sheets have a drag handle and close on swipe-down or outside tap
- Touch targets: minimum 44×44px for all interactive elements
- Enable pinch-zoom on maps, floorplans, and image viewers
- Show PWA install banner on first mobile visit (if PWA enabled)

## Tone
- No corporate jargon in UI copy
- Short, active-voice labels ("Create Draft", not "Click here to create a new draft")
- Error messages explain what happened and what to do next