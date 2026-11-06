# ROVE Snow Forecast MCP

Free, read-only [Model Context Protocol](https://modelcontextprotocol.io/) server for ski snow forecasts, opening dates, live resort conditions, and station-verified forecast accuracy across 667 ski resorts. Built by [Chufan Shi](https://www.linkedin.com/in/scf001008), founder of [ROVE](https://rove-ski.com/en).

- **Endpoint:** `https://rove-ski.com/mcp` (Streamable HTTP)
- **Coverage:** 667 ski resorts (644 in North America, 23 in Japan Beta). 15-day snowfall forecasts for all resorts; live snow depth, lifts, trails, roads, opening dates, resort news, and station-verified accuracy records for North America.
- **Auth:** OAuth 2.1 (PKCE + Dynamic Client Registration) on `https://rove-ski.com/mcp` with zero API key for Claude, Cursor, VS Code, Codex, and similar clients. Optional free API keys from [rove-ski.com/en/ai](https://rove-ski.com/en/ai) as `Authorization: Bearer <key>` or `?key=<key>` for headless / CI.
- **Official MCP Registry:** `com.rove-ski/snow-forecast` ([registry entry](https://registry.modelcontextprotocol.io/v0.1/servers/com.rove-ski%2Fsnow-forecast/versions/latest))

Docs, OAuth setup, and optional API keys: [https://rove-ski.com/en/ai](https://rove-ski.com/en/ai)  
Claude Directory: [https://claude.ai/directory/rove-snow-forecast](https://claude.ai/directory/rove-snow-forecast)
Live snow map: [https://rove-ski.com/en/snow](https://rove-ski.com/en/snow)  
Compare ski forecast apps: [https://rove-ski.com/en/best-ski-forecast-app](https://rove-ski.com/en/best-ski-forecast-app)  
Example resorts: [Mammoth Mountain](https://rove-ski.com/en/r/Mammoth%20Mountain), [Alta](https://rove-ski.com/en/r/Alta), [Vail](https://rove-ski.com/en/r/Vail), [Whistler](https://rove-ski.com/en/r/Whistler)

## The 9 tools

| Tool | What it does |
|------|----------------|
| `search_resorts` | Find ski resorts by name, region, or season pass (Ikon, Epic, Indy, Mountain Collective) |
| `get_forecast` | 15-day mid-mountain snow forecast with daily and 48-hour hourly detail |
| `get_conditions` | Live snow depth, open lifts and trails, weather, and road status (North America) |
| `get_opening_dates` | 2026-27 target and confirmed opening dates and season status |
| `find_powder` | Rank resorts by forecasted snowfall over the next 1 to 15 days |
| `get_accuracy` | Two-season station-verified backtest stats where the sample threshold is met |
| `get_resort_news` | Recent official resort announcements and operations updates |
| `plan_trip` | Compare snowfall, opening status, and historical snow reliability across resorts |
| `forecast_confidence` | Model spread and confidence context for an upcoming storm window |

All tools are read-only. Every response includes a direct `rove-ski.com` resort URL for citation. Rate limits: 1,000 calls per day and 30 per minute per key.

## How accuracy is verified

ROVE blends multiple weather models at mid-mountain elevation. For North American resorts with a matched nearby snow pillow or weather station, next-day (lead-1) forecasts are checked against measured snowfall from SNOTEL, NOAA, Environment Canada, and CDEC networks over the past two seasons. Public hit-rate figures are published only when a resort has a real station match and meets the verified snow-day sample threshold. Estimated (`est`) values from nearby resorts are never published as station-verified hit rates.

## Quick install

OAuth 2.1 is the default. Most clients need no API key. Optional static keys remain available at [rove-ski.com/en/ai](https://rove-ski.com/en/ai) for headless or non-OAuth environments.

### Claude.ai (Web & Mobile)

Preferred: install from the official listing [ROVE Snow Forecast in the Claude Connectors Directory](https://claude.ai/directory/rove-snow-forecast) and click **Connect**.

Fallback: in Claude, open **Customize → Connectors → Add custom connector**, and enter `https://rove-ski.com/mcp` (OAuth handles sign-in).

### Claude Desktop

Prefer the Directory listing or a custom connector URL `https://rove-ski.com/mcp`. For config-file installs that still require a key, merge `examples/claude_desktop_config.json` (or the `mcp-remote` variant in `examples/claude_desktop_config.alt.json`) and replace `YOUR_ROVE_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http rove https://rove-ski.com/mcp -s user
```

Start `claude` and run `/mcp` once to finish browser OAuth. Headless fallback:

```bash
claude mcp add --transport http rove https://rove-ski.com/mcp \
  --header "Authorization: Bearer YOUR_ROVE_API_KEY" -s user
```

### Codex CLI / App / IDE

```bash
codex mcp add rove --url https://rove-ski.com/mcp
```

Run `codex mcp login rove` if prompted. If OAuth is disabled in your Codex config:

```bash
codex mcp add rove --url "https://rove-ski.com/mcp?key=YOUR_ROVE_API_KEY"
```

See also `examples/codex_config.toml`.

### VS Code / Copilot

```bash
code --add-mcp '{"name":"rove","type":"http","url":"https://rove-ski.com/mcp"}'
```

### Gemini CLI

```bash
gemini mcp add -s user --transport http rove https://rove-ski.com/mcp
```

Key fallback: append `?key=YOUR_ROVE_API_KEY` to the URL. See also `examples/gemini_settings.json`.

### Cursor

One-click install from [rove-ski.com/en/ai](https://rove-ski.com/en/ai#manual), or merge into `~/.cursor/mcp.json` (keep your other servers):

```json
{
  "mcpServers": {
    "rove": {
      "url": "https://rove-ski.com/mcp"
    }
  }
}
```

Cursor runs OAuth 2.1 + dynamic client registration on connect. Key fallback: use `https://rove-ski.com/mcp?key=YOUR_ROVE_API_KEY` instead.

### Grok / any Streamable HTTP MCP client

- Preferred: URL `https://rove-ski.com/mcp` with OAuth 2.1 when the client supports it
- Fallback: URL `https://rove-ski.com/mcp?key=YOUR_ROVE_API_KEY`, or header `Authorization: Bearer YOUR_ROVE_API_KEY`

## Try it

After connecting, ask:

- "Where is the deepest snow forecast in North America over the next 10 days?"
- "15-day snow forecast and lift status for Mammoth Mountain"
- "When does Alta open for 2026-27, and what is its verified SNOTEL hit rate?"

Or call `find_powder` directly with `{"days":5,"limit":5}`.

## Agent auto-install

Paste this prompt into any coding or desktop agent:

> Install ROVE ski forecasts for me: https://rove-ski.com/mcp/install.md

## Author

Created by **Chufan Shi** ([GitHub @ChufanS008](https://github.com/ChufanS008) · [LinkedIn](https://www.linkedin.com/in/scf001008)), founder of [ROVE](https://rove-ski.com/en) in Los Angeles, CA.

## License

MIT. Copyright (c) 2026 Chufan Shi (ROVE). Weather data by Open-Meteo (CC BY 4.0) and the US National Weather Service; Japan forecasts use the JMA model via Open-Meteo. Resort locations copyright OpenStreetMap contributors.
