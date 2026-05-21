# Pipeworx Perplexity Install Kit

Connect Perplexity (Pro / Max / Enterprise plans) to live data from **2,506 tools across 575 packs** — SEC filings, USPTO patents, FRED economic data, FDA drug data, Census, EPA, ATTOM real estate, weather, and 567+ more.

Backed by the [Pipeworx](https://pipeworx.io) MCP gateway at `gateway.pipeworx.io`.

## Requirements

- Perplexity **Pro, Max, or Enterprise** plan (custom MCP connectors are not available on the Free tier)
- Custom connector feature enabled in Settings (shipped 2026-03-13)

## Install

Perplexity adds custom MCP servers through the in-app UI rather than a manifest file. There's no public marketplace submission yet — every install is user-pasted.

**Step 1.** In Perplexity, go to **Settings → Connectors → Add a connector → Custom MCP server**.

**Step 2.** Fill in the fields from [`CONNECTOR_CONFIG.md`](./CONNECTOR_CONFIG.md). Short version:

| Field | Value |
|---|---|
| Name | `Pipeworx` |
| Server URL | `https://gateway.pipeworx.io/pipeworx-catalog/mcp` |
| Transport | default (Streamable HTTP) |
| Auth | none (anonymous tier) |

**Step 3.** Save. The connector should show **Connected** with ~17 tools available.

**Step 4. (Recommended)** Open the Space you want Pipeworx active in and paste the contents of [`space-instructions.md`](./space-instructions.md) into its **Custom instructions** field. This teaches Perplexity when to reach for `ask_pipeworx` / `discover_tools` instead of hand-writing facts.

## Verify after install

Try a query in any Space using the connector:

> What was the unemployment rate last month?

Perplexity should call `ask_pipeworx`, which routes to `fred_get_series`, and return a real number with a citation.

## How it works

The connector loads **17 meta-tools** from the Pipeworx gateway — not all 2,506 underlying tools. That's deliberate: dumping every tool definition into the context window burns tokens you'll never use.

Instead, Perplexity reaches for `ask_pipeworx` or `discover_tools` and the gateway routes the request to the right pack at session time. You get the full catalog without paying for it up front.

The loaded meta-tools:

- **`ask_pipeworx`** — natural-language router.
- **`discover_tools`** — top-20 most relevant tools for a task, with full schemas.
- **`entity_profile`**, **`recent_changes`**, **`compare_entities`**, **`resolve_entity`** — fan-out across multiple packs in one call.
- **`validate_claim`** — fact-check claims against SEC XBRL. Returns a verdict + citation.
- **`remember`** / **`recall`** / **`forget`** — persistent memory across sessions.
- **`list_packs`**, **`search_packs`**, **`get_pack_tools`**, **`get_connection_config`**, **`get_platform_status`**, **`search_mcp_directory`** — browse the catalog.

## Higher rate limits

The default config runs on the anonymous tier (50 calls/day per IP). For higher limits (500/day BYO, 2,000/day OAuth, or unlimited paid), [sign up at pipeworx.io](https://pipeworx.io) and add an `X-API-Key` header in the connector's Headers section with your Pipeworx API key.

## Scope: Individual vs Organization

When adding the connector, Perplexity asks whether the connector is **Individual** (private to you) or **Organization** (admins can share org-wide). Pick Organization if you want every member of your Perplexity workspace to see Pipeworx tools without re-adding the connector.

## Links

- Gateway: https://gateway.pipeworx.io
- Stack guide: https://pipeworx.io/stack
- Source: https://github.com/pipeworx-io/pipeworx

## License

MIT
