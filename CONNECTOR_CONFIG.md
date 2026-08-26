# Perplexity custom MCP connector — paste-ready fields

When you click **Add a connector** in Perplexity (Settings → Connectors → Add custom MCP server), use:

| Field | Value |
|---|---|
| Name | `Pipeworx` |
| Description | `Live data gateway: 5,635+ tools across 1,477+ sources — SEC, FDA, FRED, Census, EPA, USPTO, ATTOM, weather, and more.` |
| Server URL | `https://gateway.pipeworx.io/oauth/mcp` |
| Transport | leave default (Streamable HTTP) |
| Icon | leave default — or upload `assets/icon.png` from this repo when available |
| Scope | `Individual` (private to you) or `Organization` (admins can share org-wide) |
| Auth | **OAuth** (expand *Advanced*) — sign in with GitHub. Free, and it is the difference between 50 calls a day and 200. Registration is automatic; you do not need to create a client yourself. |

**No account?** Use `https://gateway.pipeworx.io/pipeworx-catalog/mcp` with
**Auth: None** — the anonymous tier, 50 calls a day per IP, no credentials.

**Already have a Pipeworx API key?** Keep the anonymous URL above and add a
header — a key sent to the OAuth URL is rejected, because that endpoint
authenticates with a bearer token:

| Header name | Value |
|---|---|
| `X-API-Key` | your Pipeworx API key (BYO key 200/day) — get one at https://pipeworx.io |

After saving, the connector should show **Connected** with ~38 tools visible. Try a query in any Space:

> What was the unemployment rate last month?

Perplexity should call `ask_pipeworx`, which routes to `fred_get_series`.
