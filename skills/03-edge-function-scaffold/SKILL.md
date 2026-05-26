---
name: edge-function-scaffold
description: Use when adding a new Supabase edge function, background agent, cron task, webhook handler, or API endpoint to any project. Ensures every function meets security, reliability, and observability standards from the start.
---

# Edge Function Scaffold

Every edge function in the Holistic Edge workspace must follow this pattern without exception.

## Authentication (Required)
- Use `supabase.auth.getUser()` — NEVER `getSession()` for server-side auth
- Reject unauthenticated requests immediately with HTTP 401
- Validate workspace membership before accessing any workspace-scoped data

## Security Headers (Required on every response)
```
{
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',
  'X-Content-Type-Options': 'nosniff',
  'X-Frame-Options': 'DENY'
}
```

## Rate Limiting
Check `api_rate_limits` table before processing. Default: 20 requests/minute per user. Return HTTP 429 if exceeded.

## Error Handling
- Wrap all logic in try/catch
- Return structured errors: `{ error: string, code: string, ok: false }`
- Never expose stack traces or internal schema details in responses
- Remove all `console.log` — no debug logging in production

## Verification Payload (Required on every success response)
```json
{
  "ok": true,
  "function": "function-name",
  "timestamp": "2026-01-01T00:00:00Z",
  "result": { ... }
}
```

## Cron Health Check (Required for scheduled functions)
Before running any scheduled logic:
```sql
SELECT enabled FROM cron_job_settings WHERE job_name = 'your-job-name'
```
If `enabled = false`, return `{ ok: true, skipped: true, reason: "disabled by admin" }` immediately.

## Model Router (Required when calling LLMs)
Use tiered model selection based on task complexity:
- Embeddings / vector search: `text-embedding-3-small`
- Classification, extraction, simple tasks: `gpt-4o-mini`
- Strategy, synthesis, complex reasoning: `gpt-4o`
- Root cause analysis: `o3-mini`
- Log every LLM call to `token_usage_log` table: (function_name, model, prompt_tokens, completion_tokens, cost_estimate)