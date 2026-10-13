# AdBot Inspector

> **Free tool to test if your landing page passes ChatGPT Ads review.**
> Simulates OpenAI's OAI-AdsBot crawler — checks WAF blocking, JS-only shells, ad policy compliance, and ad–landing relevance **before** you submit.

🔗 **Use it now**: **[adsbot-inspector.com](https://adsbot-inspector.com)** — free, no signup, ~10 seconds per report.

---

## Why this exists

When you submit an ad on ChatGPT, OpenAI sends its **OAI-AdsBot** crawler to your landing page. If your WAF blocks the crawler, or your page is a JavaScript-only shell, or your copy trips a policy pattern — the ad gets stuck in review or silently rejected. Most advertisers find out **after** wasting a review cycle.

AdBot Inspector checks all of that **before** you submit.

> Especially relevant since ChatGPT Ads went fully **self-serve with no minimum spend** (May 2026, 40+ countries) — thousands of first-time advertisers now meet this crawler without knowing it exists.

## What it checks

| # | Check | Catches |
|---|-------|---------|
| 1 | **OAI-AdsBot crawl simulation** | WAF/CDN blocking (Cloudflare, Akamai, DataDome, Imperva), robots.txt disallows |
| 2 | **JS-only shell detection** | Client-side rendered pages that read as blank to crawlers |
| 3 | **OpenAI ad policy scan** | Prohibited categories, brand impersonation, misleading claim patterns |
| 4 | **Ad–landing relevance** | Keyword overlap between your ad copy and page content (H1 ×3 / meta ×2 / body ×1) |
| 5 | **Consistency check** | meta vs body, og:title vs title, AdsBot view vs browser view |

Every report includes a verdict (`ready` / `needs_fix` / `blocked` / `fail`), a 0–100 score, and **copy-paste fix suggestions** (including WAF-specific config snippets).

## Guides

| Guide | For |
|-------|-----|
| [OAI-AdsBot: The Complete Guide](https://adsbot-inspector.com/guide/oai-adsbot-complete-guide) | Everything OpenAI documented about the crawler, and what it means for your page |
| [ChatGPT Ads Rejected? 7 Reasons & Fixes](https://adsbot-inspector.com/guide/chatgpt-ads-rejected-fix) | The seven real causes, each with symptom/why/fix |
| [Is Cloudflare Blocking ChatGPT Ads?](https://adsbot-inspector.com/guide/cloudflare-chatgpt-ads) | How to confirm in your firewall logs + copy-paste allow rule |
| [Self-Serve Launch Checklist](https://adsbot-inspector.com/guide/chatgpt-ads-self-serve-checklist) | 10 points before your first campaign goes live |
| [Small Business: 5 First-Week Mistakes](https://adsbot-inspector.com/guide/chatgpt-ads-small-business) | Small-budget traps and the correct plays |

## API

See [`docs/API.md`](docs/API.md). The public endpoint is `POST /api/v1/inspect` — same checks as the web UI, 10 requests/hour/IP.

## Reporting issues

- 🐛 **Bug report** (wrong detection, false positive/negative) — [open an issue](../../issues/new?template=bug_report.md)
- 💡 **Feature request** — [open an issue](../../issues/new?template=feature_request.md)
- 📜 **New policy detection rule** — saw an ad get rejected for a reason we don't detect? [tell us](../../issues/new?template=new_policy_rule.md) with the rejection reason and page excerpt

Detection rules are updated as OpenAI's ad policies evolve — watch [CHANGELOG.md](CHANGELOG.md).

## FAQ

**Is this official OpenAI software?** No. Independent tool, built on OpenAI's public crawler documentation and ad policies. OpenAI and ChatGPT are trademarks of OpenAI.

**Does it store my pages?** No. Statistics record only domain names and aggregate counts — no page content, no client IPs, no personal data.

**What does it cost?** Free, 10 inspections/hour/IP. No account needed.

## License & content

- Documentation and guides in this repo: **CC BY-NC-SA 4.0**
- The detection engine itself is **not open source** (this repo hosts docs, guides, and the issue tracker)

---

*Built for advertisers meeting OAI-AdsBot for the first time. Tool: [adsbot-inspector.com](https://adsbot-inspector.com) · Feedback: [issues](../../issues)*
