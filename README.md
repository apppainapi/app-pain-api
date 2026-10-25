<div align="center">

<img src="logo.svg" width="96" alt="App Pain API logo" />

# App Pain API

**What users hate about any iOS/Android app — clustered complaints, evidence quotes, trends. One MCP server, no API key.**

[![MCP](https://img.shields.io/badge/MCP-2025--06--18-7ee0c0.svg)](https://modelcontextprotocol.io)
[![Streamable HTTP](https://img.shields.io/badge/transport-Streamable%20HTTP-7ec4ff.svg)](https://modelcontextprotocol.io/specification/2025-06-18/basic/transports)
[![No key required](https://img.shields.io/badge/install-no%20key%20required-ff5c39.svg)](#install)

[Live server](https://app-pain-api.quiet-delta.workers.dev/mcp) · [Docs](https://app-pain-api.quiet-delta.workers.dev/docs) · [OpenAPI 3.1](https://app-pain-api.quiet-delta.workers.dev/openapi.json) · [Pain index (101 apps)](https://app-pain-api.quiet-delta.workers.dev/pains) · [Pricing](https://app-pain-api.quiet-delta.workers.dev/#pricing)

</div>

---

Review intelligence for **any** app on the App Store or Google Play — not just your own. Feed it an app id, a store link, or a bare package name; get back ranked pain-point clusters mined from real 1–2★ reviews, each with its share of complaints, a severity score, and verbatim evidence.

Built for competitive research and agent workflows: one tool call replaces a week of reading reviews by hand.

```json
{
  "painPoints": [
    { "rank": 1, "title": "Intrusive ads & ad frequency", "sharePercent": 16, "severity": 4 },
    { "rank": 2, "title": "Shadowbanning & algorithm suppression", "sharePercent": 12, "severity": 4 }
  ],
  "overview": "Monetization pressure (ads) and opaque algorithm changes dominate complaints."
}
```

<sub>↑ real output — TikTok, US App Store. Cached analyses return instantly; ~100 popular apps are pre-analyzed and always free to read.</sub>

## The tools

| Tool | What it does |
|---|---|
| `search_apps` ★ | Find apps on both stores (merged results, rich fields) — the step 1 of every teardown |
| `mine_app_pain_points` ★ | **The core tool**: fetch recent reviews, keep the negative ones, cluster them into ranked pain points with evidence quotes |
| `get_pain_trend` | Compare two snapshots: what got worse, what got fixed (new / resolved / rising / falling) |
| `get_version_history` | Rating per release — catch the version that broke things |
| `get_app_reviews` | Raw reviews, unified schema across stores (dev replies, thumbsUp) |
| `get_app_details` | Metadata: rating histogram, category, price, IAP |
| `watch_competitors` | Paid: push new 1–2★ reviews and rising pain points to Slack/Discord/webhooks |

Also ships two prompts (`competitor_pain_report`, `app_pain_deep_dive`) so an agent can run a full teardown from one sentence.

## Install

Server URL — paste it into any MCP client, **no key needed**:

```
https://app-pain-api.quiet-delta.workers.dev/mcp
```

| Client | How |
|---|---|
| **Claude Code** | `claude mcp add --transport http app-pain-api https://app-pain-api.quiet-delta.workers.dev/mcp` |
| **Claude Desktop / claude.ai** | Settings → Connectors → *Add custom connector* → paste the URL |
| **ChatGPT** | Settings → Apps & Connectors → Developer mode → *Create* → paste the URL, auth: *No authentication* |
| **Cursor** | `~/.cursor/mcp.json` → `{"mcpServers":{"app-pain-api":{"url":"https://app-pain-api.quiet-delta.workers.dev/mcp"}}}` |
| **VS Code** | `code --add-mcp '{"name":"app-pain-api","type":"http","url":"https://app-pain-api.quiet-delta.workers.dev/mcp"}'` |
| **Windsurf** | `~/.codeium/windsurf/mcp_config.json` → `{"mcpServers":{"app-pain-api":{"serverUrl":"…/mcp"}}}` |
| **Codex CLI** | `~/.codex/config.toml` → `[mcp_servers.app-pain-api]` `url = "…/mcp"` |
| **Gemini CLI** | `~/.gemini/settings.json` → `{"mcpServers":{"app-pain-api":{"httpUrl":"…/mcp"}}}` |
| **stdio-only clients** | `npx -y mcp-remote https://app-pain-api.quiet-delta.workers.dev/mcp` |

**Keyless limits**: unlimited cached results plus a few new analyses per day. Want your own quota (10/month free, more on paid tiers)? Register at [`POST /v1/register`](https://app-pain-api.quiet-delta.workers.dev/docs) and add an `x-api-key` header. When the server-side allowance runs out, `mine_app_pain_points` degrades gracefully — it hands your agent the prepared negative reviews to cluster with its own model.

> **Mainland China**: `*.workers.dev` needs a system proxy or TUN mode to connect.

## Try asking your agent

- *"What do users hate most about Duolingo, and is it getting better?"*
- *"Compare the top 4 habit-tracker apps' pain points and find a product gap."*
- *"Watch my 3 biggest competitors and alert our Slack when their users start complaining about something new."*

## Also a plain REST API

Everything the MCP server does is available over REST with JSON responses and a full [OpenAPI 3.1 spec](https://app-pain-api.quiet-delta.workers.dev/openapi.json) — same cache, same quota, one `GET`:

```bash
curl -s "https://app-pain-api.quiet-delta.workers.dev/v1/apps/ios/835599320/painpoints"
```

## Pricing

Free tier forever (no credit card) · **Starter $9/mo** (any country, 500-review depth, watch 2 competitors) · **Pro $29/mo** (daily alerts for 15 competitors, unlimited analyses) — details on the [site](https://app-pain-api.quiet-delta.workers.dev/#pricing).

## Notes

- Data comes from public app-store pages, normalized into one schema. Not affiliated with Apple or Google.
- All tools are read-only (`readOnlyHint` annotations set).
- Speaks the classic `initialize` handshake (2024-11-05 → 2025-11-25) and answers the mandatory `server/discover` RPC from the 2026-07-28 spec — modern clients probe, see the supported version list, and fall back to the handshake automatically.
- Cold analyses take 30–120s and stream progress notifications; repeat calls hit a 7-day cache and return in milliseconds.
- [Privacy](https://app-pain-api.quiet-delta.workers.dev/privacy) · [Terms](https://app-pain-api.quiet-delta.workers.dev/terms) · [Acceptable use](https://app-pain-api.quiet-delta.workers.dev/aup)
- Contact: [apppainapi@proton.me](mailto:apppainapi@proton.me)
