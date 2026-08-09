# Nebula Landing Page Audit — MCP Skill

> Evidence-grade landing page audit via [Nebula Components](https://nebulacomponents.com).
> Free. No API key. Works with any MCP-compatible agent.

## What it does

Paste a URL. Get a structured diagnosis of why the page isn't converting paid traffic.

Audits 9 conversion signals:

| Signal | What it checks |
|--------|---------------|
| Headline | Outcome-specific, 12–90 chars, above fold |
| CTA | Single dominant action with outcome language |
| Social proof | Testimonial/logo/metric near first CTA |
| Message match | Ad headline reflected in page headline |
| Mobile | CTA visible at 375px without scroll |
| Load speed | Page weight within sane limits |
| SEO foundations | Title, meta description, H1 all present |
| Ad signals | UTM handling, relevance indicators |
| AI readiness | JSON-LD, OG tags, clean hierarchy |

Every finding includes: measured value, required standard, fix, effort, impact, confidence.

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

## Tools

### `run_audit(url)`

```
run_audit("https://example.com")
```

Returns score (0–10), grade (A–F), and prioritized findings.

### `compare_audits(url_a, url_b)`

```
compare_audits("https://yourpage.com", "https://competitor.com")
```

### `get_audit(audit_id)` · `recent_audits(limit)`

Retrieve past audits by ID or list recent ones.

## Example output

```
## Audit: https://example.com
Score: 6.2/10  Grade: B

**Message Match** [Quick Win]
  Issue:    Ad headline "Stop wasting ad spend" doesn't match page H1
  Fix:      Mirror ad outcome language in H1
  Measured: "We help businesses grow"
  Required: Outcome-specific headline matching ad copy
  Confidence: definitive

**CTA Clarity** [Quick Win]
  Issue:    Two competing CTAs with equal visual weight
  Fix:      Make primary CTA dominant; reduce secondary contrast
  Confidence: definitive
```

## Resources

- [Free audit tool](https://nebulacomponents.com/audit) — no agent required
- [Nebula Components](https://nebulacomponents.com) — landing page conversion optimization
- [SKILL.md](./SKILL.md) — full skill documentation for agent frameworks

## License

MIT
