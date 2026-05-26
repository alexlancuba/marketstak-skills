---
name: supabase-rls-audit
description: Use when auditing Row Level Security on Supabase tables, after adding new tables, when a user reports seeing data they should not see, or as part of any security review.
---

# Supabase RLS Audit

## Step 1 — Inventory
List every table in the database. For each table record:
- Table name
- RLS enabled? (yes / no)
- Number of SELECT / INSERT / UPDATE / DELETE policies

## Step 2 — Enable Check
Any table with RLS disabled is an immediate CRITICAL failure.
Enable RLS on every table before proceeding to policy review.

## Step 3 — Policy Review
For each RLS policy, verify:
- SELECT: filters by `auth.uid() = user_id` OR workspace_id membership
- INSERT: sets `user_id = auth.uid()` or validates workspace ownership
- UPDATE / DELETE: scoped so users cannot modify other users' rows
- No policy uses a bare `USING (true)` or `WITH CHECK (true)` without additional scoping

## Step 4 — Cross-User Access Test
Simulate the following scenario mentally and verify policies prevent it:
If user A knows user B's record ID, can user A:
- Read it via a direct SELECT? → Expected: empty result set
- Update it? → Expected: 0 rows affected
- Delete it? → Expected: 0 rows affected

## Step 5 — Service Role Check
The Supabase service role key must appear ONLY in edge functions (server-side).
Scan all files in `src/` — any import or use of the service role key there is CRITICAL.

## Step 6 — Report
Produce a summary table (as a list, not markdown table):

For each table:
- Table name | RLS on/off | Policies present | Status | Fix needed

For each FAIL, provide the exact SQL to fix it:
```sql
ALTER TABLE table_name ENABLE ROW LEVEL SECURITY;
CREATE POLICY "policy_name" ON table_name FOR SELECT USING (auth.uid() = user_id);
```