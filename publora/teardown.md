# Publora — Activation Teardown

**Author:** Ricardo Lo · **Date:** 2026-07-06 · **Company:** Publora (publora.com / app.publora.com)

> PM activation teardown — Playwright audit 2026-07-06. For Serge Bulaev.

---

## TL;DR

**Problem:** Homepage sells **"The Publishing API for the Agent Era"** (HTTPS + MCP), but post-signup drops users on a **10-platform OAuth connect wall** with social-scheduler copy — while the real API/MCP activation path (create key → curl/MCP config) is buried in the sidebar.

**Proposed fix:** Add an **intent fork at signup** ("Developer / agent" vs "Creator / scheduler"); route API-first users to **`/dashboard/api`** with a guided **create-key → copy curl → test post** flow before any OAuth. Default post-signup for PH/API traffic should not be the Channels empty state.

**Artifact:** This doc + screenshots in `./screenshots/`

---

## Evidence

| # | Step | Friction | Screenshot |
|---|------|----------|------------|
| 1 | Marketing homepage | `copy_confusion` | `screenshots/step_1b_homepage_agent_api_story.png` |
| 2 | Post-signup Channels | `integration_wall` | `screenshots/step_2_connect_accounts_wall.png` |
| 3 | Home dashboard | `white_canvas` | `screenshots/step_3_home_empty_dashboard.png` |
| 4 | API page (reachable) | `time_to_value` | `screenshots/step_4_api_key_available.png` |
| 5 | MCP page (reachable) | `time_to_value` | `screenshots/step_5_mcp_setup_available.png` |

**Step 1 — Homepage:** Hero reads **"The Publishing API for the Agent Era"** — post to 10 platforms with one HTTPS call; MCP and REST API above the fold. **"3 accounts free forever."**

**Step 2 — Signup (observed):** Left rail promises *"Connect your social accounts, schedule posts across all platforms, and start growing your audience in minutes"* — creator workflow, not API-first. Email/password signup is frictionless (no paywall).

**Step 3 — Post-signup redirect:** Lands on **`/dashboard/accounts`** with **10 Connect buttons** (Instagram, Threads, TikTok, YouTube, LinkedIn, Mastodon, Bluesky, X, Telegram, Facebook). Empty state: *"Connect a channel to get started."* **0/3 channels** on free tier.

**Step 4 — Home:** Stats all zero (connections, published, posts/week). *"No drafts yet"* with **Start writing** — no pointer to API key or MCP setup despite marketing positioning.

**Step 5 — API (sidebar):** **+ Create New Key** available **without** connecting social first; curl/Node/Python examples ready. This is the fastest path to value for agent-era buyers but is **not** the default onboarding destination.

**Step 6 — MCP (sidebar):** Cursor/Claude/Codex/OpenClaw setup chips + copy-paste `mcp.json` snippet referencing API key. Strong fit for Track 3 positioning; discoverability depends on user already knowing to look past Channels wall.

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

_Key changes:_

- Align signup copy with homepage for API traffic (or branch copy by entry URL/referrer)
- Post-signup route by intent — stop sending all users to OAuth grid
- Surface **first API key + MCP snippet** as the activation milestone for agent-era positioning
- Reduce choice overload: one recommended channel for creators, not ten equal CTAs

---

## Metric

**Primary:** Signup → **first successful API call** (or MCP tool invocation) within 24h

**Secondary:** Signup → first channel connected (creator cohort only)

**How to measure:** Funnel events `signup_complete` → `api_key_created` → `api_request_success` vs `channel_connected`; segment by onboarding intent selection.

---

## Links

| Asset | URL |
|-------|-----|
| Teardown (this doc) | https://github.com/rixter145/pm-activation-teardowns/blob/main/publora/teardown.md |
| App signup | https://app.publora.com/signup |
| API docs | https://docs.publora.com |
| Founder LinkedIn | https://www.linkedin.com/in/sbulaev |

---

## Outreach note (internal — do not paste into DM)

Draft only after public URL is live. Target: Serge Bulaev (LinkedIn or s@cccrafts.ai). Lead with API/MCP positioning mismatch + proposed intent fork — no resume flex.

```bash
python scripts/eval_outreach_value_first.py \
  --message-file Tracking/Outreach_Drafts/publora_founder_dm_2026-07-06.txt \
  --artifact-url https://github.com/rixter145/pm-activation-teardowns/blob/main/publora/teardown.md
```
