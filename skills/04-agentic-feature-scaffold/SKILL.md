---
name: agentic-feature-scaffold
description: Use when adding AI agent capabilities, autonomous workflows, background processing, or LLM-powered features to any project. Applies the Sensor → Policy → Tool → Quality → Learning pattern for agentic architecture.
---

# Agentic Feature Scaffold

Use the five-layer agent pattern for every autonomous feature.

## Layer 1: Sensor (What triggers this agent?)
- Define the trigger: HTTP webhook, DB trigger, cron schedule, or user action
- Define the input schema with types upfront
- Validate all inputs before passing to the policy layer

## Layer 2: Policy (What should happen?)
- Define the decision logic: what action to take given the input
- Implement human-in-the-loop gates using the `approval_gate_config` table:
  - `auto` — agent acts immediately
  - `notify` — agent acts and sends notification
  - `require_approval` — agent creates a pending action and waits
- Define fallback behavior for each failure mode

## Layer 3: Tool (What does the agent do?)
- Define the concrete action: write to DB, call LLM, send notification, call external API
- Every action must be logged to the relevant audit table
- Make actions idempotent where possible (safe to retry)
- Define the success signal explicitly

## Layer 4: Quality (How do we know it worked?)
Return a verification payload from every agent run:
```json
{
  "ok": true/false,
  "agent": "agent-name",
  "action": "description of what it did",
  "timestamp": "ISO-8601",
  "metadata": { ... }
}
```
Define what "wrong" looks like and how the monitor-agent will detect it.

## Layer 5: Learning (How does it improve?)
- Does this agent's output feed back into the Brain (brain-ingest)?
- Track a quality metric over time (acceptance rate, error rate, user edits)
- Feed results as signals for the strategy-refresh cycle

## Required Infrastructure for Every Agent
- [ ] Cron toggle: check `cron_job_settings` before running scheduled logic
- [ ] Rate limiting or exponential backoff on LLM calls
- [ ] Token usage logged to `token_usage_log`
- [ ] Error escalation: failures visible in `/admin/monitor`
- [ ] Manual trigger button in `/admin/cron-health` or relevant admin panel
- [ ] Brain signal emitted on meaningful outcomes (successes and failures)