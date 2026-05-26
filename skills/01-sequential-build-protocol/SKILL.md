---
name: sequential-build-protocol
description: Use when the user mentions phases, pipeline, roadmap, multi-step plan, or any task involving more than one major deliverable in sequence. Also use when the request contains words like "proceed", "next step", "implement phase", or "build it". Do NOT execute multiple phases in one go without explicit per-phase approval.
---

# Sequential Build Protocol

## Rule
Never execute more than one phase or major deliverable at a time without explicit approval from the user.

## Steps
1. List all phases/steps clearly before building anything
2. Present the plan as a numbered list with a one-line description of each phase
3. Stop and wait for explicit approval before starting Phase 1
4. After each phase completes, stop and report what was built — do not continue
5. Wait for the user to say "proceed", "yes", "go", "next", or equivalent before starting the next phase
6. If the user changes scope mid-sequence, pause, re-outline the updated plan, and get approval again

## What counts as a "phase"
- Any feature with 3+ distinct components (DB + UI + edge function)
- Any item labeled "Phase N", "Step N", or "Part N" in the conversation
- Any work that touches more than one page or edge function
- Any task where a mistake in step 1 would waste the effort in step 2

## Never do
- Start Phase 2 while Phase 1 is running
- Combine phases to "save time" without explicit user consent
- Run parallel builds on the same project
- Interpret "yes" or "proceed" to mean "do all remaining phases at once"