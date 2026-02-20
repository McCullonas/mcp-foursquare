# mcp-foursquare

MCP server for Foursquare place search and discovery. Lets your AI assistant find venues,
search for places near a location, look up place details, and get location context — all
through the Foursquare Places API v3.

**Status:** Ready

---

## Prerequisites

- [Bun](https://bun.sh) runtime (`curl -fsSL https://bun.sh/install | bash`)
- A [Foursquare Developer](https://location.foursquare.com/developer/) account and API key

---

## Install

```bash
git clone git@github.com:McCullonas/mcp-foursquare.git
cd mcp-foursquare
bun install
```

---

## Configuration

The server requires one environment variable:

| Variable | Required | Description |
|----------|----------|-------------|
| `FOURSQUARE_SERVICE_TOKEN` | Yes | Foursquare v3 API token from your developer dashboard |

Get your API key at [location.foursquare.com/developer](https://location.foursquare.com/developer/).
Free tier includes 1,000 calls/day.

---

## Available Tools

### `search_near`
Search for places near a named location (city, neighbourhood, landmark).

**Parameters:**
- `query` — What to search for (e.g. `"coffee"`, `"museum"`)
- `near` — Named location (e.g. `"Edinburgh, UK"`, `"Soho, London"`)
- `categories` *(optional)* — Foursquare category IDs to filter by
- `limit` *(optional)* — Max results (default 10)

### `search_near_point`
Search for places near a specific latitude/longitude coordinate.

**Parameters:**
- `query` — What to search for
- `ll` — Coordinates as `"lat,lng"` (e.g. `"51.5074,-0.1278"`)
- `radius` *(optional)* — Search radius in metres
- `limit` *(optional)* — Max results

### `place_snap`
Get the most likely place at a given location — useful for geotagging.

**Parameters:**
- `ll` — Coordinates as `"lat,lng"`
- `accuracy` *(optional)* — GPS accuracy in metres

### `place_details`
Get comprehensive details about a specific place by its Foursquare ID.

Returns: description, phone, website, hours, rating, price level, menu link, photos, tips.

**Parameters:**
- `fsq_id` — Foursquare place ID (from a search result)

### `get_location`
Get an approximate location based on the user's IP address (via ip-api.com). Useful as a
starting point when no explicit location is provided.

No parameters required.

---

## MCP Client Configuration

Add to your `settings.json` (or `claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "foursquare": {
      "command": "bun",
      "args": ["run", "/path/to/mcp-foursquare/src/index.ts"],
      "env": {
        "FOURSQUARE_SERVICE_TOKEN": "your-token-here"
      }
    }
  }
}
```

Replace `/path/to/mcp-foursquare` with the absolute path to your clone.

---

## Development

```bash
# Run directly
bun run src/index.ts

# Build (outputs to dist/)
bun build src/index.ts --outdir dist --target node
```

---

## Licence

MIT
