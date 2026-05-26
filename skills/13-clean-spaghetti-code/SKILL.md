---
name: clean-spaghetti-code
description: Use when a component, page, or file has grown too large, has duplicated logic, is difficult to understand, or has been flagged as hard to maintain. Refactors without changing any behavior.
---

# Clean Spaghetti Code

## Absolute Rule
Behavior must be identical before and after this refactor. No new features, no behavior changes, no scope creep.

## Step 1 — Audit the Target File(s)
Read the file(s) and identify:
- [ ] Components over 200 lines → extract sub-components
- [ ] Logic duplicated in 2+ places → extract to a shared hook or utility
- [ ] JSX nested more than 3 levels deep → flatten with early returns or sub-components
- [ ] Inline styles that should be Tailwind classes
- [ ] Magic numbers or hardcoded strings → extract to named constants
- [ ] API calls directly inside components → move to custom hooks
- [ ] `useEffect` with more than 3 responsibilities → split into focused effects

## Step 2 — Plan Before Touching Code
List every extraction you will make:
- "Extract [component name] from [file] into [new file]"
- "Consolidate [function A] and [function B] into [hook name]"
- "Move [API call] into [hook name]"
Do not start writing code until the plan is complete.

## Step 3 — Refactor
- One extraction at a time
- Preserve the exact same props and API surface — callers must not need to change
- Preserve all existing functionality including edge cases and error handling
- No new npm dependencies

## Step 4 — Verify
After refactoring:
- [ ] The page/feature renders identically to before
- [ ] No new TypeScript or linting errors introduced
- [ ] All existing functionality still works (happy path + edge cases)
- [ ] Every extracted file is smaller and more focused than the original
- [ ] The original file is meaningfully shorter