---
name: nebula-landing-page-audit
version: "1.0.0"
description: >
  Evidence-grade landing page audit via Nebula's MCP server.
  Detects conversion leaks across 9 signals: headline, CTA, social proof,
  message match, mobile layout, load speed, SEO foundations, ad signals,
  and AI readiness. Every finding includes measured value, required
  standard, delta, CSS selector, confidence level, and a prioritized fix.
author: Nebula Components
license: MIT
homepage: https://nebulacomponents.com
mcp_server: https://mcp.nebulacomponents.com/mcp
tags:
  - landing-page
  - conversion-rate-optimization
  - cro
  - audit
  - paid-ads
  - performance
---

# Nebula Landing Page Audit Skill

Use this skill to diagnose why a landing page isn't converting, especially
when paid traffic is already running. It runs an evidence-grade 9-signal
audit and returns a prioritized fix list — not opinions.

## When to use this skill

- "Why aren't my ads converting?"
- "Audit this landing page"
- "Find conversion leaks on [URL]"
- "Compare my page to a competitor's"
- "What's wrong with this page's CTA / headline / social proof?"

## MCP Server

This skill is backed by a live remote MCP server:

```
https://mcp.nebulacomponents.com/mcp
```

The server requires no API key. All audits are free.

## Available Tools

### `run_audit(url: str)`

Run a full evidence-grade audit against any public URL.

**Returns:** Score (0–10), letter grade (A–F), and structured findings.
Each finding includes:
- `label` — the signal being tested
- `quadrant` — Quick Win / Major Project / Fill In / Hard Slog (effort × impact)
- `issue` — what was observed
- `fix` — the specific remediation
- `evidence` — measured value, required standard, delta
- `impact` / `effort` — numeric scores (0–10)
- `confidence` — `definitive` / `high` / `contextual`

**Example:**
```
run_audit("https://example.com")
```

**Example output:**
```
## Audit: https://example.com
Score: 6.2/10  Grade: B

Findings (4):

**Message Match** [Quick Win]
  Issue:    Ad headline "Stop wasting ad spend" doesn't match page headline "We help businesses grow"
  Fix:      Mirror the ad's outcome language in the H1
  Measured: "We help businesses grow"
  Required: Outcome-specific headline matching ad copy
  Delta:    Mismatch detected

**CTA Clarity** [Quick Win]
  Issue:    Two competing CTAs above the fold with equal visual weight
  Fix:      Make one CTA dominant; reduce contrast on secondary action
  Confidence: definitive
```

---

### `compare_audits(url_a: str, url_b: str)`

Audit two pages and compare them side by side.

Use this to benchmark your page against a competitor's, or compare
a variant before committing to a redesign.

**Example:**
```
compare_audits("https://yourpage.com", "https://competitor.com")
```

---

### `get_audit(audit_id: str)`

Retrieve a completed audit by its UUID. Use this to re-examine a previous
audit without re-running it.

**Example:**
```
get_audit("a1b2c3d4-e5f6-7890-abcd-ef1234567890")
```

---

### `recent_audits(limit: int = 10)`

List the most recent audits in the Nebula database. Returns URL, score,
grade, and timestamp. Useful for surveying what's been audited.

---

## Scoring Reference

| Score | Grade | Interpretation |
|-------|-------|----------------|
| 9.0–10 | A+ | Optimized — minimal leaks |
| 8.0–8.9 | A  | Strong — 1–2 fixable issues |
| 7.0–7.9 | B  | Decent — conversion friction present |
| 6.0–6.9 | C  | Moderate leaks — fix before scaling spend |
| 5.0–5.9 | D  | Significant leaks — ads are wasting money |
| < 5.0  | F  | Critical — page is not ready for paid traffic |

## Signal Definitions

| Signal | What it measures |
|--------|-----------------|
| `headline` | Outcome-specific headline ≥ 12 chars, ≤ 90 chars, above fold |
| `cta` | Single dominant CTA with action + outcome language |
| `social_proof` | Testimonial, logo, metric, or guarantee near first CTA |
| `message_match` | Ad headline language reflected in page headline |
| `mobile` | Responsive viewport, CTA visible on 375px without scroll |
| `load_speed` | Page weight and request count within sane limits |
| `seo_foundations` | Title tag, meta description, single H1 all present |
| `ad_signals` | UTM param handling, landing page relevance score indicators |
| `ai_readiness` | JSON-LD, OG tags, clean hierarchy for AI citation |

## Usage patterns

**Basic audit:**
```
Please audit this page: https://example.com/pricing
```

**Competitive analysis:**
```
Compare our landing page to our top competitor:
- Ours: https://oursite.com/lander
- Theirs: https://competitor.com/landing
```

**Pre-launch check:**
```
We're about to run Google Ads to https://example.com/new-campaign.
Run an audit and tell me what to fix before we spend money.
```

**Quick Win filter:**
```
Audit https://example.com and list only the Quick Win findings
(high impact, low effort) so we can ship fixes today.
```

## MCP Configuration

### Claude Desktop

Add to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "nebula": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://mcp.nebulacomponents.com/mcp"
      ]
    }
  }
}
```

### Hermes Agent

```bash
hermes mcp add nebula --url https://mcp.nebulacomponents.com/mcp
```

### Any MCP-compatible client

```
Transport: streamable-http
Endpoint:  https://mcp.nebulacomponents.com/mcp
Auth:      None required
```

## Evaluation

### Test case 1 — Basic audit

**Prompt:** `Run an audit on https://nebulacomponents.com`

**Expected:**
- Returns score between 0–10
- Returns grade letter
- Returns at least 1 finding with label, issue, fix fields
- Completes in < 30 seconds

### Test case 2 — Findings structure

**Prompt:** `Audit https://example.com and list all Quick Win findings`

**Expected:**
- Agent calls `run_audit`
- Agent filters findings where `quadrant == "Quick Win"`
- Response includes `fix` for each finding

### Test case 3 — Comparison

**Prompt:** `Compare https://nebulacomponents.com and https://stripe.com/payments`

**Expected:**
- Agent calls `compare_audits` with both URLs
- Response includes scores for both pages
- Response identifies which page scores higher and why

### Test case 4 — Error handling

**Prompt:** `Audit https://localhost/private`

**Expected:**
- Tool returns an error or low-confidence result
- Agent does not hallucinate a score

## Notes

- Audits typically complete in 5–15 seconds
- The tool fetches and analyzes the live page at time of call
- Password-protected or Cloudflare-blocked pages may return limited findings
- All audits are free — no rate limits for reasonable usage
