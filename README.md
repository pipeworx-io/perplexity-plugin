# Pipeworx for Perplexity

Give Perplexity one MCP that reaches **5,635+ live-data tools across 1,477+ sources** — SEC filings, USPTO patents, FRED, Census, FDA, EPA, USAspending, Polymarket, Zillow, weather, and 1,469+ more — answered with structured data + citations instead of prose.

## Requirements

- Perplexity **Pro, Max, or Enterprise** plan (custom MCP connectors are not available on Free)
- Custom connector feature enabled in Settings (shipped 2026-03-13)

## Install

Perplexity adds custom MCP servers through the in-app UI; there's no marketplace submission yet — every install is user-pasted.

**Step 1.** **Settings → Connectors → Add a connector → Custom MCP server**.

**Step 2.** Fill in:

| Field | Value |
|---|---|
| Name | `Pipeworx` |
| Server URL | `https://gateway.pipeworx.io/oauth/mcp` |
| Transport | default (Streamable HTTP) |
| Auth | **OAuth** (under *Advanced*) — sign in with GitHub, free, 200 calls/day |

Prefer no account? Use `https://gateway.pipeworx.io/pipeworx-catalog/mcp` with
**Auth: None** instead — 50 calls a day per IP, no signup.

**Step 3.** Save. Connector should show **Connected** with ~38 tools.

**Step 4. (Recommended)** Paste [`space-instructions.md`](./space-instructions.md) into the **Custom instructions** field of any Space where you want Pipeworx active. This teaches Perplexity when to reach for `ask_pipeworx` / `discover_tools` instead of hand-writing facts.

Full field reference: [`CONNECTOR_CONFIG.md`](./CONNECTOR_CONFIG.md).

## Try it

After install, ask Perplexity in a connected Space:

| Ask | What it triggers |
|---|---|
| *"What just happened to Apple?"* | `sec_8k_recent` → SEC 8-K events classified by severity |
| *"Spread between Polymarket and Kalshi on the next Fed decision?"* | `polymarket_kalshi_spread` → live cross-venue mispricing |
| *"Overdue Phase 3 readouts at Moderna?"* | `pharma_pipeline_catalysts` → biotech catalyst calendar |
| *"DoD cybersecurity contracts this week?"* | `usa_award_search` → sub-second USAspending mirror |
| *"Median home value and renter share in Lubbock, TX?"* | `housing_market_snapshot` + `housing_metro_demand` |
| *"Unemployment rate last month?"* | `fred_get_series` → official FRED data |

Perplexity picks the right tool via `ask_pipeworx`. Every response carries `pipeworx://` citations.

## How it loads light

The connector exposes **~31 meta-tools**, not all 5,635+ — `ask_pipeworx({question})` and friends route at runtime so you get the full catalog without paying the context tax.

## Free tier + signup

**Signing in is free and takes one GitHub click** — it moves you from 50 calls a day to 200, on a stable account that does not rotate with your IP. Set the Server URL to `https://gateway.pipeworx.io/oauth/mcp` and pick **OAuth** under Advanced, or [sign up first](https://pipeworx.io/signup?via=perplexity_plugin).

No account at all still works: `https://gateway.pipeworx.io/pipeworx-catalog/mcp` with Auth: None, anonymous, 50 calls a day per IP.

## Verify after install

In a connected Space:

> What was the unemployment rate last month?

You should see a real number with a `pipeworx://` citation.

## What's loaded

- **`ask_pipeworx`** — natural-language router across all 1,477+ sources.
- **`discover_tools`** — top-20 relevant tools for a task, with full schemas.
- **`entity_profile`** / **`compare_entities`** / **`recent_changes`** / **`resolve_entity`** — fan-out across multiple packs in one call.
- **`validate_claim`** — fact-check claims against SEC XBRL.
- **`remember`** / **`recall`** / **`forget`** — persistent memory across sessions.
- **`list_packs`** / **`search_packs`** / **`get_pack_tools`** / **`get_connection_config`** / **`get_platform_status`** / **`search_mcp_directory`** — browse the catalog.

## Bring your own key

Already have a Pipeworx API key? Keep the **anonymous** URL
(`.../pipeworx-catalog/mcp`) and add an `X-API-Key` header in the connector's
Headers section — that is the BYO tier, 200/day.

Signing in is a separate route to the same 200/day and needs no key: use the
OAuth URL and pick OAuth under Advanced. The two do not combine — an
`X-API-Key` sent to `/oauth/mcp` is rejected, because that endpoint
authenticates with a bearer token.

## Scope: Individual vs Organization

When adding the connector, Perplexity asks whether it's **Individual** (private to you) or **Organization** (admins can share org-wide). Pick Organization if you want every Perplexity workspace member to see Pipeworx tools without re-adding it.

## Links

- Gateway: https://gateway.pipeworx.io
- Status: https://pipeworx.io/status
- Source: https://github.com/pipeworx-io/pipeworx

## License

MIT

---

⭐ Star if you'd use this — helps other Perplexity users discover it.
