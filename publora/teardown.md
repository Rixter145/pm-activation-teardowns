# Publora — Activation Teardown

**Publora** · publora.com · July 2026

---

## TL;DR

**Problem:** Homepage sells **"The Publishing API for the Agent Era"** (HTTPS + MCP), but post-signup drops users on a **10-platform OAuth connect wall** with social-scheduler copy — while the real API/MCP activation path (create key → curl/MCP config) is buried in the sidebar.

**Proposed fix:** Add an **intent fork at signup** ("Developer / agent" vs "Creator / scheduler"); route API-first users to **`/dashboard/api`** with a guided **create-key → copy curl → test post** flow before any OAuth. Default post-signup for PH/API traffic should not be the Channels empty state.

---

## Evidence

| Step | What happened | Screenshot |
|------|---------------|------------|
| 1 | Homepage hero promises one HTTPS call to 10 platforms; MCP and REST API above the fold | [step_1b_homepage_agent_api_story.png](./screenshots/step_1b_homepage_agent_api_story.png) |
| 2 | Signup sidebar frames creator workflow ("connect social accounts, schedule posts") — not API-first | — |
| 3 | Post-signup redirect lands on Channels page with 10 OAuth connect buttons and empty state | [step_2_connect_accounts_wall.png](./screenshots/step_2_connect_accounts_wall.png) |
| 4 | Home dashboard shows zero connections, zero published, no drafts — no pointer to API/MCP path | [step_3_home_empty_dashboard.png](./screenshots/step_3_home_empty_dashboard.png) |
| 5 | API page lets you create a key without connecting social first — not the default onboarding route | [step_4_api_key_available.png](./screenshots/step_4_api_key_available.png) |
| 6 | MCP setup with Cursor/Claude/Codex config is available in sidebar — requires finding API page first | [step_5_mcp_setup_available.png](./screenshots/step_5_mcp_setup_available.png) |

---

## User story

As a **builder evaluating Publora for agent publishing**, I land on a homepage about **one API call to 10 platforms**. I sign up in under a minute, then hit a **wall of 10 OAuth connects** and an empty channel list. I do not know I can skip straight to **API → Create Key → MCP config** without connecting Instagram first — so I stall before ever sending a test post.

---

## Proposed flow

### Before (current)

1. Homepage → API/agent positioning
2. Signup → social-scheduler sidebar copy
3. Redirect → `/dashboard/accounts` OAuth grid
4. Empty home until ≥1 channel connected
5. API/MCP available but unguided in sidebar

### After (proposed)

1. Homepage CTA unchanged
2. Signup → **"How will you use Publora?"** — **Build with API/MCP** | **Schedule from dashboard**
3. **API path** → `/dashboard/api` modal: create key → copy curl with placeholder platforms → link to docs for channel IDs
4. **Creator path** → `/dashboard/accounts` with **connect 1 channel to start** (not 10-up front)
5. Home empty state → contextual card: *"No channels yet — paste this curl to test API"* OR *"Connect LinkedIn to schedule first post"*

**Key changes:**

- Align signup copy with homepage for API traffic (or branch copy by entry URL/referrer)
- Post-signup route by intent — stop sending all users to OAuth grid
- Surface **first API key + MCP snippet** as the activation milestone for agent-era positioning
- Reduce choice overload: one recommended channel for creators, not ten equal CTAs

---

## Metric

**Primary:** Signup → **first successful API call** (or MCP tool invocation) within 24h

**Secondary:** Signup → first channel connected (creator cohort only)

**How to measure:** Funnel events signup complete → api key created → api request success vs channel connected; segment by onboarding intent selection

---

## Links

| Resource | URL |
|----------|-----|
| Signup | https://app.publora.com/signup |
| API docs | https://docs.publora.com |
