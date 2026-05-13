# Perplexity custom MCP connector — paste-ready fields

When you click **Add a connector** in Perplexity (Settings → Connectors → Add custom MCP server), use:

| Field | Value |
|---|---|
| Name | `Pipeworx` |
| Description | `Live data gateway: 1,423+ tools across 392+ packs — SEC, FDA, FRED, Census, EPA, USPTO, ATTOM, weather, and more.` |
| Server URL | `https://gateway.pipeworx.io/pipeworx-catalog/mcp` |
| Transport | leave default (Streamable HTTP) |
| Icon | leave default — or upload `assets/icon.png` from this repo when available |
| Scope | `Individual` (private to you) or `Organization` (admins can share org-wide) |
| Auth | None — gateway accepts the anonymous tier (50 calls/day per IP) without credentials |

For higher rate limits, add a header:

| Header name | Value |
|---|---|
| `X-API-Key` | your Pipeworx API key (BYO 500/day, OAuth 2,000/day, Paid unlimited) — get one at https://pipeworx.io |

After saving, the connector should show **Connected** with ~17 tools visible. Try a query in any Space:

> What was the unemployment rate last month?

Perplexity should call `ask_pipeworx`, which routes to `fred_get_series`.
