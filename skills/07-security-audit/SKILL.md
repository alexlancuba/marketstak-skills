---
name: security-audit
description: Use when performing a security review, preparing for a compliance audit, or before a major release. Runs a 7-phase end-to-end security audit and returns a severity-rated report.
---

# Security Audit — 7 Phases

Rate every finding: CRITICAL 🔴 / HIGH 🟠 / MEDIUM 🟡 / LOW 🟢 / PASS ✅

## Phase 1 — Secrets Exposure
- Scan all client-side code (`src/`) for API keys, tokens, passwords, or secrets
- Check `.env` handling — no secrets accessible in the browser bundle
- Verify no credentials in code comments or hardcoded fallback values
- Check that service role key is NEVER imported into any `src/` file

## Phase 2 — RLS Policies
- List all Supabase tables
- CRITICAL if RLS is disabled on any table
- For each policy: verify it filters by `auth.uid()` or workspace_id
- Flag any policy with a bare `true` or missing WHERE clause as CRITICAL
- Verify no direct table access bypasses policies via service role in frontend

## Phase 3 — Edge Function Auth
- Every edge function must call `getUser()` before any processing
- Flag any function that skips auth as CRITICAL
- Verify HMAC signatures on all webhook endpoints
- Confirm rate limiting is enforced on public-facing functions

## Phase 4 — Authentication
- Verify auth redirect URL whitelist (no wildcard or localhost)
- Check session and token expiry settings
- Test: can user A access user B's data by changing record IDs in requests?
- Confirm admin-only routes enforce role checks server-side, not just client-side

## Phase 5 — Frontend Vulnerabilities
- Check for XSS: all user-generated content rendered as text, not HTML
- Verify no `dangerouslySetInnerHTML` used with untrusted input
- Confirm no sensitive data (tokens, keys, PII) stored in localStorage
- Check Content Security Policy headers are set

## Phase 6 — Infrastructure
- All `console.log` statements removed from production edge functions
- Error responses do not expose stack traces, schema names, or internal state
- CORS restricted to appropriate origins
- Security headers present on all edge function responses

## Phase 7 — GDPR / Compliance
- Privacy policy exists, is accurate, and is linked from the app
- Terms of service linked
- User data deletion path exists
- No unnecessary PII logged or retained

**Security Score: X/7 phases passed**
List all findings with severity rating and specific remediation steps.