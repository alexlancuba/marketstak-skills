---
name: deep-debug
description: Use when diagnosing a bug, error, unexpected behavior, or when the user says something is broken, not working, or wrong. Do not write any fix until the root cause is identified and stated explicitly.
---

# Deep Debug — 6-Phase Process

## Absolute Rule
No code changes until Phase 5 is complete and a root cause is stated.

## Phase 1 — Reproduce
- Confirm the exact steps to reproduce the issue
- Note: browser, device, auth state, specific user or workspace triggering it
- Is it 100% reproducible or intermittent?
- When did it start? (correlate with recent changes)

## Phase 2 — Isolate the Layer
Determine which layer owns the bug (check only one at a time):
- Frontend rendering or React state
- API call or network response
- Edge function logic
- Supabase query or RLS policy
- Auth / session handling
- Cron or background job timing

## Phase 3 — Inspect Evidence
- Read the relevant component and hook files
- Read the relevant edge function
- Check the Supabase table schema and RLS policies
- Review recent edit history for changes that correlate with the bug onset

## Phase 4 — State the Root Cause
Before touching any code, state the root cause in one sentence:
> "The bug occurs because [X] when [Y], causing [Z]."

If you cannot complete this sentence with confidence, go back to Phase 3.

## Phase 5 — Validate the Hypothesis
Identify what would confirm or deny the hypothesis without writing a fix:
- Check a specific log, network response, or DB query result
- Trace the data flow from input to output

## Phase 6 — Fix
- Make the minimal change that addresses the root cause
- Do not refactor or improve unrelated code in the same pass
- Verify the fix does not break adjacent functionality
- Report: what was wrong, what was changed, and how to confirm it's fixed