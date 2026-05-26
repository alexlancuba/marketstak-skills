---
name: post-generation-security-audit
description: Use immediately after generating any new feature, page, edge function, or database table. Verifies that all security invariants were properly implemented and nothing was accidentally left open.
---

# Post-Generation Security Audit

Run this after every significant code generation. Return PASS ✅ or FAIL ❌.

## Authorization
- [ ] Every new edge function calls `supabase.auth.getUser()` and rejects unauthenticated requests with 401
- [ ] New database queries are scoped to the authenticated user's ID or workspace
- [ ] No new admin-only actions are accessible to regular users
- [ ] Permissions are enforced server-side — client-side checks are UI only, never security

## Input Validation
- [ ] All user inputs validated before passing to database or LLM
- [ ] No raw user input passed to SQL (all queries use parameterized values, not string concatenation)
- [ ] File uploads (if any) validated for allowed MIME type and maximum size
- [ ] No user-supplied content reflected back without sanitization

## Secrets & Keys
- [ ] No new API keys, tokens, or passwords appear anywhere in `src/` files
- [ ] All external service credentials use environment variables
- [ ] No secrets appear in code comments, console.log calls, or default values

## Data Exposure
- [ ] API responses include only the fields the client actually needs
- [ ] Error responses do not expose schema names, stack traces, or internal state
- [ ] No PII (name, email, phone) appears in logs or error messages

## RLS for New Tables
- [ ] RLS is enabled on every newly created table
- [ ] SELECT / INSERT / UPDATE / DELETE policies are all defined
- [ ] Policies tested: user A cannot read, update, or delete user B's records

**Security invariants intact: YES / NO**
If NO: list every failing item with the exact file/function and the specific fix required.