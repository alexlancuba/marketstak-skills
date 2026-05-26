---
name: pre-launch-checklist
description: Use when a project is about to go live, be published, or be shown to real users for the first time. Also use before sharing a demo link with investors, clients, or press. Returns a pass/fail report — do not launch until all items pass.
---

# Pre-Launch Checklist

Return PASS ✅ or FAIL ❌ for each item. Include a one-line note for every FAIL. Do not approve launch until all items are PASS.

## Security
- [ ] No hardcoded API keys, tokens, or secrets in client-side code or comments
- [ ] No `sk_test_` or `pk_test_` Stripe keys in production environment
- [ ] RLS enabled on ALL Supabase tables (zero exceptions)
- [ ] Every RLS policy scoped to `auth.uid()` or workspace_id — no bare `true` conditions
- [ ] All edge functions reject unauthenticated requests
- [ ] Rate limiting active on all public-facing endpoints
- [ ] Security headers present: X-Frame-Options, X-Content-Type-Options

## Authentication
- [ ] Login, signup, and logout flows work end-to-end in incognito
- [ ] Auth redirect URLs point to production domain (no localhost)
- [ ] Password reset / forgot password flow works
- [ ] Email confirmation flow works (if enabled)
- [ ] Admin-only routes inaccessible to regular users

## Content & UX
- [ ] Zero placeholder copy ("Lorem ipsum", "TODO", "Your name here", "Coming soon")
- [ ] No broken navigation links or 404s on main paths
- [ ] All forms have validation and clear error states
- [ ] Empty states handled gracefully (no blank white screens)
- [ ] Loading states exist on all async operations

## Mobile
- [ ] Tested at 375px width (iPhone SE) — no horizontal overflow
- [ ] All touch targets ≥ 44px
- [ ] Bottom navigation visible and functional on mobile
- [ ] Core user journey completable on mobile

## SEO & Discovery
- [ ] Page `<title>` and meta description set for all public pages
- [ ] Favicon uploaded
- [ ] Social preview image (OG image) set
- [ ] Open Graph tags present on marketing pages

## Infrastructure
- [ ] All required production environment variables set in Supabase
- [ ] No dev/test URLs or localhost references in production code
- [ ] Analytics events firing on core user actions
- [ ] Privacy Policy page exists and is linked in footer
- [ ] Terms of Service page exists and is linked in footer

## Final Smoke Test
- [ ] Tested in incognito (logged-out state looks correct)
- [ ] Tested with a fresh account (onboarding flow completes without errors)
- [ ] Admin panel verified: accessible to admin, inaccessible to regular user

**Launch approved: YES / NO**
If NO: list every FAIL item with the specific fix required.