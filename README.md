# ROVE Snow Forecast MCP

Free, read-only [Model Context Protocol](https://modelcontextprotocol.io/) server for ski snow forecasts, opening dates, live resort conditions, and station-verified forecast accuracy across 667 ski resorts. Built by [Chufan Shi](https://www.linkedin.com/in/scf001008), founder of [ROVE](https://rove-ski.com/en).

- **Endpoint:** `https://rove-ski.com/mcp` (Streamable HTTP)
- **Coverage:** 667 ski resorts (644 in North America, 23 in Japan Beta). 15-day snowfall forecasts for all resorts; live snow depth, lifts, trails, roads, opening dates, resort news, and station-verified accuracy records for North America.
- **Auth:** Free API key from [rove-ski.com/en/ai](https://rove-ski.com/en/ai) passed as `Authorization: Bearer <key>` or `?key=<key>` (OAuth 2.0 dynamic client registration also supported for web MCP clients)
- **Official MCP Registry:** `com.rove-ski/snow-forecast` ([registry entry](https://registry.modelcontextprotocol.io/v0.1/servers/com.rove-ski%2Fsnow-forecast/versions/latest))

Docs and free API key: [https://rove-ski.com/en/ai](https://rove-ski.com/en/ai)  
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

### Claude.ai (Web & Mobile)

Preferred: install from the official listing [ROVE Snow Forecast in the Claude Connectors Directory](https://claude.ai/directory/rove-snow-forecast).

Fallback: in Claude Settings, open **Connectors**, click **Add custom connector**, and enter `https://rove-ski.com/mcp` to connect via OAuth.

### Claude Desktop

Merge `examples/claude_desktop_config.json` into your Claude Desktop MCP config (or use the `mcp-remote` variant in `examples/claude_desktop_config.alt.json`). Replace `YOUR_ROVE_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http rove https://rove-ski.com/mcp \
  --header "Authorization: Bearer YOUR_ROVE_API_KEY" -s user
```

### ChatGPT Desktop / Codex CLI

```bash
codex mcp add rove --url "https://rove-ski.com/mcp?key=YOUR_ROVE_API_KEY"
```

See also `examples/codex_config.toml`.

### Gemini CLI

```bash
gemini mcp add -s user --transport http rove \
  "https://rove-ski.com/mcp?key=YOUR_ROVE_API_KEY"
```

See also `examples/gemini_settings.json`.

### Cursor

Merge `examples/cursor_mcp.json` into `~/.cursor/mcp.json` (keep your other servers):

```json
{
  "mcpServers": {
    "rove": {
      "url": "https://rove-ski.com/mcp?key=YOUR_ROVE_API_KEY"
    }
  }
}
```

### Grok / any Streamable HTTP MCP client

Point your MCP client at:

- URL: `https://rove-ski.com/mcp?key=YOUR_ROVE_API_KEY`
- or URL `https://rove-ski.com/mcp` with header `Authorization: Bearer YOUR_ROVE_API_KEY`

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
