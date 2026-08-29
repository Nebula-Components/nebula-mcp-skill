# Nebula Landing Page Audit — MCP Skill

> Evidence-grade landing page audit via [Nebula Components](https://nebulacomponents.com).
> Free. No API key. Works with any MCP-compatible agent.

## What it does

Paste a URL. Get a structured diagnosis of why the page isn't converting paid traffic.

Audits 9 conversion signals:

| Signal | What it checks |
|--------|----------------|
| Headline | Outcome-specific, 12-90 chars, above fold |
| CTA | Single dominant action with outcome language |
| Social proof | Testimonial/logo/metric near first CTA |
| Message match | Ad headline reflected in page headline |
| Mobile | CTA visible at 375px without scroll |
| Load speed | Page weight within sane limits |
| SEO foundations | Title, meta description, H1 all present |
| Ad signals | UTM handling, relevance indicators |
| AI readiness | JSON-LD, OG tags, clean hierarchy |

Every finding includes: measured value, required standard, fix, effort, impact, confidence.

## Benchmark data

From 293 completed audits (Q3 2026):
- **62%** of pages fail the headline test (H1 doesn't match the ad that sent the visitor)
- **39%** of pages are missing social proof near the CTA
- **40%** fail the load speed check
- Average page has **2.7 conversion leaks**

Full data: [nebulacomponents.com/research/landing-page-performance-q3-2026](https://nebulacomponents.com/research/landing-page-performance-q3-2026)

## MCP Server

```
https://mcp.nebulacomponents.com/mcp
```

Transport: `streamable-http` · Auth: none required

## Quick setup

### Claude Desktop

```json
{
  "mcpServers": {
    "nebula": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.nebulacomponents.com/mcp"]
    }
  }
}
```

### Hermes Agent

```bash
hermes mcp add nebula --url https://mcp.nebulacomponents.com/mcp
```

### Cursor / VS Code

Add to your MCP settings:
```json
{
  "nebula": {
    "url": "https://mcp.nebulacomponents.com/mcp"
  }
}
```

## Tools

### `run_audit(url)`

```
run_audit("https://example.com")
```

Returns score (0-10), grade (A-F), and prioritized findings.

### `compare_audits(url_a, url_b)`

```
compare_audits("https://yourpage.com", "https://competitor.com")
```

### `get_fix_instructions(audit_id)`

```
get_fix_instructions("audit-uuid-here")
```

Returns machine-readable repair steps for AI coding agents (Claude Code, Cursor, Aider).

### `get_audit(audit_id)` · `recent_audits(limit)`

Retrieve past audits by ID or list recent ones.

## Example output

```
## Audit: https://example.com
Score: 6.2/10  Grade: B

**Message Match** [Quick Win]
Issue: Ad headline "Stop wasting ad spend" doesn't match page H1
Fix: Mirror ad outcome language in H1
Measured: "We help businesses grow"
Required: Outcome-specific headline matching ad copy
Confidence: definitive

**CTA Clarity** [Quick Win]
Issue: Two competing CTAs with equal visual weight
Fix: Make primary CTA dominant; reduce secondary contrast
Confidence: definitive
```

## Resources

- [Free audit tool](https://nebulacomponents.com/audit) — no agent required
- [How Nebula audits](https://nebulacomponents.com/how-nebula-audits) — the 9-signal methodology
- [Q3 2026 Landing Page Research](https://nebulacomponents.com/research/landing-page-performance-q3-2026) — benchmark data from 293 audits
- [Wikidata entity](https://www.wikidata.org/wiki/Q141206192) — Q141206192

## License

MIT
