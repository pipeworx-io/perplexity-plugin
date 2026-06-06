# Pipeworx for Perplexity

Give Perplexity one MCP that reaches **3,300+ live-data tools across 750+ sources** — SEC filings, USPTO patents, FRED, Census, FDA, EPA, USAspending, Polymarket, Zillow, weather, and 740+ more — answered with structured data + citations instead of prose.

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
| Server URL | `https://gateway.pipeworx.io/pipeworx-catalog/mcp` |
| Transport | default (Streamable HTTP) |
| Auth | none (anonymous tier) |

**Step 3.** Save. Connector should show **Connected** with ~26 tools.

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

The connector exposes **~26 meta-tools**, not all 3,300+ — `ask_pipeworx({question})` and friends route at runtime so you get the full catalog without paying the context tax.

## Free tier + signup

100 calls/day anonymous, IP-bound. [Sign up free in 10s via GitHub](https://pipeworx.io/signup?via=perplexity_plugin) for 2,000/day + a stable account.

## Verify after install

In a connected Space:

> What was the unemployment rate last month?

You should see a real number with a `pipeworx://` citation.

## What's loaded

- **`ask_pipeworx`** — natural-language router across all 750+ sources.
- **`discover_tools`** — top-20 relevant tools for a task, with full schemas.
- **`entity_profile`** / **`compare_entities`** / **`recent_changes`** / **`resolve_entity`** — fan-out across multiple packs in one call.
- **`validate_claim`** — fact-check claims against SEC XBRL.
- **`remember`** / **`recall`** / **`forget`** — persistent memory across sessions.
- **`list_packs`** / **`search_packs`** / **`get_pack_tools`** / **`get_connection_config`** / **`get_platform_status`** / **`search_mcp_directory`** — browse the catalog.

## Bring your own key

For BYO-tier (500/day) or OAuth (2,000/day), add an `X-API-Key` header in the connector's Headers section.

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
