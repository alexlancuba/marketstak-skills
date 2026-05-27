# Skill #26 — MTP Agent Constitution

**When to use:** Any project where AI agents make autonomous decisions on behalf of an organization. Apply at project inception, or retrofit before scaling agent autonomy.

## What it is

The Massive Transformative Purpose (MTP) pattern, drawn from Salim Ismail's *Exponential Organizations* and Organizational Singularity framework, turns an organization's purpose into a **constitutional constraint** that every agent must respect. It is not background context — it is a hard, non-negotiable mandate injected into every system prompt, paired with explicit boundary conditions the agent will refuse to violate.

## Why it matters

As agentic systems gain autonomy, alignment becomes the bottleneck. Prompt engineering per-task does not scale. Without a constitution:
- Agents drift toward the median of their training data, not your organization's purpose
- "Tone" instructions get diluted across long chains
- Different agents make contradictory decisions because none share a north star
- There is no checkable guardrail — only vibes

An MTP + boundary conditions gives agents a single, ambitious purpose ("Make world-class marketing accessible to every founder on earth") and a list of `Never/Always` rules they treat as veto conditions. Every output is filtered through it.

---

## Step-by-step implementation

### 1. Database — add MTP columns

```sql
ALTER TABLE public.brand_profiles
  ADD COLUMN IF NOT EXISTS mtp text,
  ADD COLUMN IF NOT EXISTS mtp_boundary_conditions jsonb NOT NULL DEFAULT '[]'::jsonb;
```

For projects without a `brand_profiles` table, attach the columns to whichever table represents the org/workspace identity (e.g. `workspaces`, `organizations`).

### 2. Shared helper — `_shared/mtp.ts`

Drop this into your edge-function shared folder. It loads the MTP for a profile/workspace/user and formats it as a system-prompt preamble.

```ts
export interface MtpData {
  mtp: string | null;
  boundary_conditions: string[];
}

export async function fetchMtp(
  admin: any,
  opts: { profileId?: string | null; workspaceId?: string | null; userId?: string | null },
): Promise<MtpData | null> {
  let row: any = null;
  try {
    const base = admin.from("brand_profiles").select("mtp, mtp_boundary_conditions");
    if (opts.profileId) {
      row = (await base.eq("id", opts.profileId).maybeSingle()).data;
    } else if (opts.workspaceId) {
      row = (await base.eq("workspace_id", opts.workspaceId)
        .order("updated_at", { ascending: false }).limit(1).maybeSingle()).data;
    } else if (opts.userId) {
      row = (await base.eq("user_id", opts.userId)
        .order("updated_at", { ascending: false }).limit(1).maybeSingle()).data;
    }
  } catch (_) { /* MTP is optional */ }
  if (!row) return null;
  const mtp = typeof row.mtp === "string" ? row.mtp.trim() : "";
  if (!mtp) return null;
  const bc = Array.isArray(row.mtp_boundary_conditions)
    ? row.mtp_boundary_conditions.filter((s: any) => typeof s === "string" && s.trim().length > 0)
    : [];
  return { mtp, boundary_conditions: bc };
}

export function formatMtpBlock(data: MtpData | null): string {
  if (!data || !data.mtp) return "";
  const conds = data.boundary_conditions.length
    ? data.boundary_conditions.map((c) => `- ${c}`).join("\n")
    : "- (none specified — use the MTP itself as the sole guardrail)";
  return `=== ORGANIZATIONAL MANDATE (MTP) ===
Massive Transformative Purpose: ${data.mtp}

Boundary Conditions (hard constraints — never violate these):
${conds}

All outputs must remain within these boundaries. If a proposed action conflicts with the MTP or any boundary condition, flag it and propose an alternative instead.
===

`;
}

export function withMtp(systemPrompt: string, data: MtpData | null): string {
  const block = formatMtpBlock(data);
  return block ? block + systemPrompt : systemPrompt;
}
```

### 3. Inject into every agent edge function

```ts
import { fetchMtp, withMtp } from "../_shared/mtp.ts";

const mtp = await fetchMtp(admin, { workspaceId });
const systemPrompt = withMtp(BASE_SYSTEM_PROMPT, mtp);

await callLLM({ system: systemPrompt, messages });
```

Apply this to **every** agent: content generation, strategy synthesis, optimization, monitoring, experiment design. No agent is exempt — that is the point.

The injected block always uses this exact format so agents recognize it across functions:

```
=== ORGANIZATIONAL MANDATE (MTP) ===
Massive Transformative Purpose: <one sentence>

Boundary Conditions (hard constraints — never violate these):
- Never ...
- Always ...

All outputs must remain within these boundaries. If a proposed action conflicts with the MTP or any boundary condition, flag it and propose an alternative instead.
===
```

### 4. UI — `MtpSection` component

A dedicated section in the Brand/Workspace settings page. Two fields:
- **MTP Statement** — single textarea, one sentence, encourages bold framing
- **Boundary Conditions** — `TagInput` of `Never…` / `Always…` rules

Visual treatment: primary-tinted card, `Shield` icon, an "Active · N guardrails" badge once filled, and an explanatory footer that re-states "Hard constraint, not context." Pair both fields with `AISuggestButton` so users with weak first drafts can bootstrap.

### 5. UI — `MtpBadge` component

A small pill rendered on agentic dashboards (e.g. `Brain`, `YC Metrics`, agent activity views). Two states:
- **Set:** `Shield` icon, `MTP Active · N guardrails`
- **Unset:** `ShieldAlert` icon, `MTP not set · configure` — links to settings

It loads the active workspace's MTP once and caches per render. Its purpose is ambient reassurance: the user can see at a glance that the constitution is live.

### 6. Onboarding integration

Add a dedicated "Purpose & guardrails" step to the onboarding wizard, ideally **after** the brand basics step and **before** the first agent-driven action (first content generation, first strategy synthesis). Make it optional but strongly encouraged — show 2-3 example MTPs (Google, Tesla, your own product) and a placeholder boundary condition. Users who skip can be re-prompted by the `MtpBadge` in the unset state.

### 7. Why hard constraint, not context

This is the conceptual move that makes the pattern work.

**Context** is information the model *may* use to influence an answer. It competes with every other instruction, example, and prior token. Stuff enough context into the prompt and the original purpose decays.

**A constraint** is a rule the model *must* not violate, framed in the same imperative register as a refusal policy. By prefixing the system prompt with `=== ORGANIZATIONAL MANDATE ===`, using imperative language ("never violate", "flag conflicts and propose alternatives"), and listing boundary conditions as `Never/Always` rules, you signal: *this is not background — this is a veto.*

Empirically:
- Models comply with constraint-framed instructions far more reliably than context-framed ones
- Boundary conditions starting with `Never`/`Always` become checkable post-hoc by a verifier agent
- A short, ambitious MTP outperforms a long brand-voice document for alignment, because the agent can hold it in working memory across long chains
- When the agent *does* flag a conflict and propose an alternative (instead of silently violating), you get a free audit trail of edge cases worth reviewing

The MTP is the closest thing an agentic system has to a constitution. Treat it that way.

---

## Files in the companion zip

- `SKILL.md` — this document
- `_shared/mtp.ts` — drop-in helper
- `migrations/add_mtp_columns.sql` — sample migration
- `components/MtpSection.tsx` — settings UI
- `components/MtpBadge.tsx` — dashboard pill

## Verification checklist

- [ ] Columns exist on the org/workspace table
- [ ] `fetchMtp` + `withMtp` imported by **every** agentic edge function
- [ ] At least one boundary condition test: ask the agent to do something that violates a rule, confirm it refuses and proposes an alternative
- [ ] `MtpBadge` rendered on primary agentic dashboards
- [ ] Onboarding step exists and is reachable from the `MtpBadge` unset state
