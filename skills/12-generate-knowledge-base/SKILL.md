---
name: generate-knowledge-base
description: Use when onboarding to a project, before a major refactor, when the codebase has grown complex and needs documentation, or when a new team member joins. Analyzes the entire project and produces a structured reference document.
---

# Generate Project Knowledge Base

Analyze the entire project codebase and produce a structured knowledge document. Save it as `PROJECT_KNOWLEDGE.md` at the project root.

## Document Structure

### 1. Product Overview
- What the app does (2–3 sentences maximum)
- Primary user type(s) and their core job-to-be-done
- Core value proposition in one sentence

### 2. Tech Stack
- Frontend framework and key libraries used
- Backend: Supabase tables count, edge functions count
- Authentication method and providers
- External APIs and services connected (Stripe, Resend, OpenAI, etc.)

### 3. Database Schema
For each table:
- Table name
- Purpose (one sentence)
- Key columns with types
- RLS status (enabled / disabled)
- Relationships to other tables

### 4. Edge Functions Index
For each edge function:
- Function name
- Trigger type (HTTP POST, cron schedule, DB webhook)
- Purpose (one sentence)
- Auth required (yes / no)
- Cron schedule (if applicable)

### 5. Page / Route Map
List every route in the app with what it renders and who can access it.

### 6. Key Business Logic
Document non-obvious logic that is not clear from reading the code:
- Pricing rules and tier logic
- Approval workflows and gate conditions
- Role hierarchies and permission matrix
- State machines (booking status, content status, etc.)

### 7. Design System Reference
- Primary colors and usage
- Component patterns in use
- Mobile breakpoints and behavior

### 8. Known Limitations & Technical Debt
List anything that is a workaround, incomplete, or flagged for future attention.

### 9. Environment Variables Required
List all environment variables needed to run the app in production (keys only, no values).

### 10. Recent Changes (Last 5 Significant Edits)
Summarize the last 5 meaningful changes made to the project.