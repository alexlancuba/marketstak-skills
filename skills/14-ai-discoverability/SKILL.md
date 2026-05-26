---
name: ai-discoverability
description: Use when optimizing a project to be found and cited by AI search engines and assistants such as ChatGPT, Perplexity, Claude, and Gemini. Generates llms.txt, structured data, and an agent reference file.
---

# AI Discoverability Setup

Make the app visible, accurate, and citable by AI assistants and search engines.

## Step 1 — Generate /llms.txt
Create `public/llms.txt`:
- Product name and one-sentence description
- Primary use case
- Target user type
- Key features as a bullet list
- Public pricing tiers (if applicable)
- Links to key public pages

## Step 2 — Generate /llms-full.txt
Extended version with:
- Full feature descriptions
- Step-by-step guides for core user workflows
- FAQ section covering common questions
- Integration and API capabilities

## Step 3 — Structured Data (JSON-LD)
Add to the root `<head>` of every public page:
- `Organization` schema: name, URL, logo, sameAs social profiles
- `SoftwareApplication` schema: name, applicationCategory, description, offers (pricing)

## Step 4 — Agent API Reference
Create `public/agent-api.md`:
- Available API endpoints with method, path, and purpose
- Authentication method
- Key data models with field descriptions
- Example request and response for each endpoint

## Step 5 — robots.txt
Ensure `public/robots.txt`:
- Explicitly allows: `GPTBot`, `ClaudeBot`, `PerplexityBot`, `Googlebot`
- Blocks known malicious scrapers
- Disallows `/admin`, `/api`, and all auth routes for crawlers

## Step 6 — Sitemap
Verify `sitemap.xml` exists at `/sitemap.xml` and includes all public-facing pages with correct `lastmod` dates.

## Step 7 — Verify
After completing all steps, confirm:
- `https://yourdomain.com/llms.txt` returns the file
- `https://yourdomain.com/robots.txt` shows correct allow/disallow rules
- `https://yourdomain.com/sitemap.xml` lists all public pages