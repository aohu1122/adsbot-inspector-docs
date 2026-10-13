# Changelog

All notable changes to AdBot Inspector. Dates are UTC.

## 2026-10

- **2026-10-08**: Public docs repo launched (README, guides, API reference, issue templates).
- **2026-10-05**: Five guides published (OAI-AdsBot complete guide, 7 rejection reasons, Cloudflare/WAF, self-serve checklist, small business mistakes). On-page SEO overhaul (title/meta/OG/Twitter/JSON-LD, canonical, robots.txt, sitemap).
- **2026-10-05**: Custom domain `adsbot-inspector.com` bound to the Cloudflare Worker.
- **2026-10-02**: D1-backed persistent statistics (cross-isolate); rate limiting (10/h/IP) verified active on Workers; multiple Pyodide runtime fixes (threadpool, env bindings, epoch timestamps); the `/inspect` endpoint fully functional on the Workers deployment for the first time.
- **2026-10-02**: Railway deployment retired; Cloudflare Workers is the sole production backend.
- **2026-09-30**: Statistics module (in-memory, backend) with admin dashboard.

## Detection engine history

The engine's rule set (18 prohibited categories, 15 misleading-claim patterns, 8 usage-policy classes, three severity tiers) was calibrated against OpenAI's public ad policies and a 50-site real-world test sweep. Rule updates follow OpenAI policy changes — see issues labeled `policy-rule`.
