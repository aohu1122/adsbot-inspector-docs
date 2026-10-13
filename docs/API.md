# AdBot Inspector — API Reference

Base URL (production): `https://adsbot-inspector.com`

All endpoints are public unless marked otherwise. Rate limit: **10 requests/hour/IP** (token bucket).

---

## POST `/api/v1/inspect` — main inspection

Runs the full check pipeline: concurrent crawl (OAI-AdsBot UA / browser UA / OAI-SearchBot UA / robots.txt) → text extraction → block detection → JS-shell detection → policy scan → relevance scoring → consistency check → report.

### Request body

| Field | Type | Required | Notes |
|---|---|---|---|
| `url` | string | ✅ | Landing page URL (http/https). Private/localhost ranges rejected (SSRF guard). |
| `ad_title` | string | — | Your ad title (feeds relevance scoring) |
| `ad_body` | string | — | Your ad body copy |
| `context_hints` | string | — | Comma-separated targeting hints |
| `include_comparison` | bool | — | Include AdsBot-vs-browser comparison (default true) |

### Response (200) — `InspectReport`

```
{
  "url": "...",
  "verdict": "ready | needs_fix | blocked | fail",
  "summary": "...",
  "top_actions": ["..."],            // prioritized fix list
  "scores": { "crawl": 0-100, "policy": 0-100, "relevance": 0-100, "overall": 0-100 },
  "crawl": { ... },                  // per-UA statuses, robots results, redirect chain
  "block_detection": { ..., "fix_guide": { ... } },  // WAF evidence + paste-ready fixes
  "policy_scan": { "matches": [ { rule_id, severity, suggestion, ... } ] },
  "relevance": { "score": 0-100, "missing_keywords": [...] },
  "context_hints_csv": "...",        // suggested hints generated from the page
  "disclaimer": "..."
}
```

Verdict priority: `blocked` > `fail` (critical policy hit) > `needs_fix` (warnings / JS shell / robots block) > `ready`.

### Errors

| Code | Meaning |
|---|---|
| 400 | validation error |
| 422 | malformed body (Pydantic-level) |
| 429 | rate limited (10/h/IP) |
| 500 | crawl/internal failure |

### Example

```bash
curl -X POST https://adsbot-inspector.com/api/v1/inspect \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com","ad_title":"Example Domain","ad_body":"illustrative examples"}'
```

---

## GET `/api/v1/inspect/{report_id}` — cached report

Reports are cached 10 minutes per URL hash; a report id is issued per inspection.

---

## GET `/api/v1/policy-rules` — active rule set

Returns the prohibited categories (with severity tiers), impersonation and compliance rule overview currently in effect.

---

## GET `/api/v1/health`

`{"status":"healthy","timestamp":"...","version":"..."}`

---

## Internal (token-authenticated)

`GET /api/v1/stats` and `POST /api/v1/stats/reset` — aggregate usage statistics. Require `X-Admin-Token` header; not part of the public API.

---

## Notes

- The report is **rule-based static detection** and may differ from OpenAI's actual review (which weighs context and intent). Use it as a pre-flight check, not a guarantee.
- Crawler timeouts: connect 5s / read 10s / total 15s; body cap 2MB; max 5 redirects.
