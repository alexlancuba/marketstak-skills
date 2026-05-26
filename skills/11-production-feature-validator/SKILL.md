---
name: production-feature-validator
description: Use after shipping any significant new feature to validate it before announcing it to users. Triple-checks UI consistency, functional correctness, auth enforcement, and that no existing features were broken.
---

# Production Feature Validator

Run this after every significant feature ship. Return PASS ✅ or FAIL ❌.

## 1. UI / UX Consistency
- [ ] New UI matches the Holistic Edge design system (teal, glassmorphism, typography)
- [ ] Mobile layout correct at 375px — no overflow, no broken layout
- [ ] Loading state exists for all async operations
- [ ] Empty state handled (no blank screen when there is no data)
- [ ] Error state handled (clear message, not a blank screen or unhandled exception)
- [ ] No visual regressions on pages adjacent to the new feature

## 2. Functional Correctness
- [ ] Happy path works end-to-end (logged in, valid inputs, expected output)
- [ ] Edge cases: empty input, maximum length input, special characters
- [ ] Async operations complete correctly — no silent failures
- [ ] Data persists correctly after page refresh
- [ ] Data is scoped correctly to the user's workspace

## 3. Auth & Permissions
- [ ] Feature is inaccessible to logged-out users (if it should be)
- [ ] Feature is inaccessible to users without the required role
- [ ] No cross-workspace data visible or accessible
- [ ] Admin-only features gated correctly

## 4. Edge Functions (if applicable)
- [ ] Function returns verification payload `{ ok: true, function: "...", timestamp: "..." }`
- [ ] Function handles missing or invalid input with a clear error response
- [ ] Rate limiting respected (20 req/min default)
- [ ] Cron health check integrated (if scheduled)
- [ ] Token usage logged (if LLM is used)

## 5. Regression Check
Identify the 3 features most likely affected by this change and verify they still work:
1. [Adjacent feature 1] — PASS / FAIL
2. [Adjacent feature 2] — PASS / FAIL
3. [Adjacent feature 3] — PASS / FAIL

**Feature approved for users: YES / NO**
If NO, list specific items to fix before announcing.