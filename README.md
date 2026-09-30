# How To Make Money On Snapchat: remote MCP server

HowToMakeMoneyOnSnapchat.com runs a hosted, read-only MCP server with five tools. Two tools return Snap's published creator-monetization rules and rule changes, each with its source and verified date. Three tools run deterministic calculations on metrics the caller supplies: eligibility against the published criteria, the numeric gap, and the pace needed to close it. Results are sourced facts or calculations, never an official Snap decision.

> HowToMakeMoneyOnSnapchat.com is an independent third party. **Not affiliated with, endorsed by, or sponsored by Snap Inc.** Snapchat is a trademark of Snap Inc.

## This repository holds documentation only

The server is hosted at the URL below. This repository contains no server code, no packages, and nothing to install or run. It exists so directories that ask for a repository link have a public page describing the hosted server. No software is distributed here, so there is no license file.

## Connection

| | |
|---|---|
| Endpoint | `https://howtomakemoneyonsnapchat.com/mcp` |
| Transport | Streamable HTTP |
| Protocol version | `2025-11-25` |
| Account | Not required |
| Anonymous access | No key needed: 60 requests per 60 seconds and 5000 per day |
| Free API key (optional) | 600 requests per 60 seconds and 50000 per day. Issue one with `POST https://howtomakemoneyonsnapchat.com/v1/keys`. No email is collected. Send it as `Authorization: Bearer <key>` or `X-API-Key: <key>`. Issuing a key is limited to 5 per client per day. |

All five tools are read-only. The caller supplies every metric.

## Tools

Tool names as returned by `tools/list`, with descriptions condensed from the same response:

| Tool | Type | What it does |
|---|---|---|
| `get_monetization_requirements` | FACT | Snap's published creator-monetization rules: thresholds, country lists, policy text, source URL, verified_at, and effective date. |
| `get_rule_changes` | FACT | What changed in Snap's monetization rules since a date: previous value, new value, source, and the date the change was detected. Requires `since`. |
| `check_eligibility` | CALCULATION | Compares one creator's caller-supplied metrics with the published entry requirements. Returns `meets_published_criteria`, `does_not_meet_published_criteria`, or `unknown`, plus pass/fail/unknown per requirement. Requires `metrics`. |
| `calculate_gap` | CALCULATION | The exact numeric shortfall, percent complete, and binding constraint for a creator's caller-supplied metrics. Requires `metrics`. |
| `calculate_growth_pace` | CALCULATION | The daily and weekly pace to close each numeric gap within `target_days`, with scenarios and labeled assumptions. A projection, not a prediction. Requires `metrics` and `target_days`. |

## Connect

### Clients that use an `mcpServers` JSON file with a `url` entry (for example Cursor's `mcp.json`)

```json
{
  "mcpServers": {
    "howtomakemoneyonsnapchat": {
      "url": "https://howtomakemoneyonsnapchat.com/mcp"
    }
  }
}
```

With an optional free key:

```json
{
  "mcpServers": {
    "howtomakemoneyonsnapchat": {
      "url": "https://howtomakemoneyonsnapchat.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_FREE_KEY"
      }
    }
  }
}
```

### VS Code (`.vscode/mcp.json`)

```json
{
  "servers": {
    "howtomakemoneyonsnapchat": {
      "type": "http",
      "url": "https://howtomakemoneyonsnapchat.com/mcp"
    }
  }
}
```

### Claude Code

```bash
claude mcp add --transport http howtomakemoneyonsnapchat https://howtomakemoneyonsnapchat.com/mcp
```

### Apps that add remote servers from a settings screen

Where an app offers "add custom connector" or "add remote MCP server", paste `https://howtomakemoneyonsnapchat.com/mcp`. No OAuth or API key is needed.

### Plain HTTP

```bash
curl -sS -X POST https://howtomakemoneyonsnapchat.com/mcp \
  -H 'content-type: application/json' \
  -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Pricing

The five tools are free to call anonymously within the rate limit above, and the free API key raises that limit. The site's [pricing page](https://howtomakemoneyonsnapchat.com/pricing) says paid plans are coming soon and has no checkout.

## Links

- Website: https://howtomakemoneyonsnapchat.com
- MCP page: https://howtomakemoneyonsnapchat.com/snapchat-mcp
- Docs (MCP, curl, Python, TypeScript quickstart): https://howtomakemoneyonsnapchat.com/docs
- Official MCP Registry entry (`com.howtomakemoneyonsnapchat/creator-monetization`): https://registry.modelcontextprotocol.io/v0.1/servers/com.howtomakemoneyonsnapchat%2Fcreator-monetization/versions/latest
- MCP server card: https://howtomakemoneyonsnapchat.com/.well-known/mcp/server-card.json
- REST API and OpenAPI spec: https://howtomakemoneyonsnapchat.com/snapchat-monetization-api and https://howtomakemoneyonsnapchat.com/openapi.json
- Sources: https://howtomakemoneyonsnapchat.com/sources
- Status: https://howtomakemoneyonsnapchat.com/status
- Terms: https://howtomakemoneyonsnapchat.com/terms · Privacy: https://howtomakemoneyonsnapchat.com/privacy

## Contact

hello@howtomakemoneyonsnapchat.com

---

HowToMakeMoneyOnSnapchat.com is an independent third party and is not affiliated with, endorsed by, or sponsored by Snap Inc. Snapchat is a trademark of Snap Inc.
