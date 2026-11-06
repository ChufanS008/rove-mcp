# ROVE Snow Forecast MCP

Free, read-only ski intelligence for 667 ski resorts in the US, Canada, and Japan. Built by [Chufan Shi](https://www.linkedin.com/in/scf001008), founder of [ROVE](https://rove-ski.com/en).

- **Version:** 1.0.2
- **Endpoint:** `https://rove-ski.com/mcp` (Streamable HTTP)
- **Instant access:** No API key or sign-in. Anonymous read-only requests: 120 requests/min per IP.
- **Optional higher quota:** OAuth 2.1 with PKCE and Dynamic Client Registration, or a free API key, for 600 requests/min.
- **Official MCP Registry:** `com.rove-ski/snow-forecast`
- **Setup:** [rove-ski.com/en/ai](https://rove-ski.com/en/ai)
- **Claude Directory:** [ROVE Snow Forecast](https://claude.ai/directory/rove-snow-forecast)

## The 12 advertised tools

| Tool | Purpose |
|---|---|
| `search_resorts` | Structured catalog search by name, region, pass, or country. |
| `get_forecast` | Daily and available 3-hourly snowfall, rain, temperature, and wind forecasts up to 15 days. |
| `get_conditions` | Current snow depth, recent snowfall, and available operator lift or trail status. |
| `get_opening_dates` | Available opening dates, closing targets, and season operating status. |
| `find_powder` | Rank resorts over the next 1 to 15 days starting today. |
| `get_accuracy` | Eligible station-verified historical accuracy, or the 435-station backtest methodology. |
| `get_resort_news` | Available resort announcements and storm or weather highlights. |
| `plan_trip` | Rank resorts for an explicit calendar window from `start` to `end`. |
| `get_forecast_confidence` | Available model agreement, forecast spread, and horizon uncertainty. |
| `compare_resorts` | Compare 2 to 5 named resorts across snowfall, snowpack, lift status, pass, and eligible accuracy. |
| `search` | OpenAI-compatible keyword discovery across resorts and regional or pass hubs. |
| `fetch` | Read an internal ROVE document by canonical identifier or ROVE URL. |

Pass filters include `epic`, `ikon`, `indy`, `mountain-collective`, `power-pass`, and `independent`. Membership reflects the published catalog, not a guarantee of ticket entitlement. Check pass restrictions with the operator.

Legacy tool names remain callable but are omitted from discovery: `forecast_confidence`, `get_resort`, `get_pass_resorts`, `get_snow_rankings`, `get_worth_going`, `get_forecast_accuracy`, `get_season_snow`, and `get_weather_stations`.

## Quick install

Start with the bare endpoint. No account or API key is required.

### Claude Code

```sh
claude mcp add --transport http rove https://rove-ski.com/mcp -s user
```

### Codex

```sh
codex mcp add rove --url https://rove-ski.com/mcp
```

### VS Code / Copilot

```sh
code --add-mcp '{"name":"rove","type":"http","url":"https://rove-ski.com/mcp"}'
```

### Cursor

Merge this server into your existing MCP configuration without replacing other servers:

```json
{
  "mcpServers": {
    "rove": { "url": "https://rove-ski.com/mcp" }
  }
}
```

### Claude, ChatGPT, and other remote MCP clients

Use `https://rove-ski.com/mcp` with authentication set to none when supported. The [Claude Directory listing](https://claude.ai/directory/rove-snow-forecast) is also available. If a client requires authentication, select optional OAuth 2.1. Client account requirements and availability are controlled by that client.

### Optional authenticated access

For 600 requests/min, use OAuth 2.1 when supported or get a free API key at [rove-ski.com/en/ai](https://rove-ski.com/en/ai). Supply `Authorization: Bearer YOUR_ROVE_API_KEY`; a `?key=` URL is also supported, but a header keeps keys out of URLs and URL logs. Never publish your credentials.

## Data and provenance

Forecast availability varies by resort and time. Missing or stale values remain unavailable, not zero. Operator status is not inferred from a scheduled opening date. Resort-level hit rates are shown only when at least 10 actual snow days and real station evidence exist. Estimated data and Japan resorts do not receive published resort hit-rate figures.

The historical verification ledger covers 435 stations across mixed networks, including 105 USDA NRCS SNOTEL stations. It is not a claim that all 435 are SNOTEL stations or that every resort has a matched station. See `get_accuracy` for methodology.

Every tool response includes `canonicalUrl`, `observedAt`, attribution, JSON text content, and structured content. Cite ROVE and preserve upstream citations. `search` returns `id`, `title`, `url`, and `text`; pass a returned ID to `fetch`. Fetch accepts internal catalog documents only and never retrieves arbitrary caller-supplied URLs.

## Try it

- "Compare Mammoth Mountain and Palisades Tahoe for the next five days."
- "Find powder in Tahoe starting today."
- "Plan a ski trip for a specific date range and explain forecast uncertainty."

## Links

[Agent installation guide](https://rove-ski.com/mcp/install.md) | [Privacy](https://rove-ski.com/en/privacy) | [Terms](https://rove-ski.com/en/terms) | [Demo walkthrough](https://rove-ski.com/mcp/demo-walkthrough.mp4)

## License

MIT. Copyright (c) 2026 Chufan Shi (ROVE). Weather data by Open-Meteo (CC BY 4.0) and public weather and snowpack networks. Japan forecasts use JMA model inputs via Open-Meteo. Resort locations include OpenStreetMap contributors.
