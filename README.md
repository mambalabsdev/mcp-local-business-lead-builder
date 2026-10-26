# Local Business Lead Builder MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/com.mambabuilt/mcp-local-business-lead-builder)](https://lightnow.ai/servers/com.mambabuilt/mcp-local-business-lead-builder) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-local-business-lead-builder/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-local-business-lead-builder) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-local-business-lead-builder%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-local-business-lead-builder&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-local-business-lead-builder)](https://www.npmjs.com/package/@mambalabsdev/mcp-local-business-lead-builder) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-local-business-lead-builder)](https://www.npmjs.com/package/@mambalabsdev/mcp-local-business-lead-builder) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-local-business-lead-builder)](https://github.com/mambalabsdev/mcp-local-business-lead-builder/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-local-business-lead-builder)

An MCP server for the Mamba Labs Local Business Lead Builder actor on Apify. Give it a business category and a place, or a search such as dentist in Austin, TX. Each run reads the public Google Maps search results and returns one row per business with website, phone, address, rating, and review count.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give `queries` (for example plumber in Austin, TX), or `categories` plus `locations` to search every pair. You get one row per business: name, Google place ID and Maps link, website and domain, phone in local and international format, full address with city, postal code, state, and country, latitude and longitude, categories, star rating, review count, and weekly hours. Each search ends with one free summary row with its counts. Filters for rating, reviews, website, and phone drop businesses before they are billed, and a business found by two searches is returned once. All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-local-business-lead-builder": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-local-business-lead-builder"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `build_local_business_leads` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find 50 roofing contractors in Denver, CO with a website and at least 4 stars."
- "List dentists in Manchester, UK with their phone numbers and review counts."
- "Build a lead list of HVAC contractors in Austin, Dallas, and Houston, Texas."

## Tool

`build_local_business_leads`. One tool. Example arguments:

```json
{
  "categories": ["roofing contractor"],
  "locations": ["Denver, CO"],
  "max_places_per_query": "50",
  "require_website": "true",
  "min_rating": "4.0"
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and booleans are sent as strings.

| Input | Type | Description |
|---|---|---|
| `queries` | array | Searches, for example dentist in Austin, TX. Up to 100 |
| `categories` | array | Business categories, searched in every location |
| `locations` | array | Places: a city with its state or country, a ZIP code, or a neighborhood |
| `max_places_per_query` | string | Place rows per search, 1 to 500. Default 20 |
| `min_rating` | string | Keep businesses rated at or above this |
| `min_reviews` | string | Keep businesses with at least this many reviews |
| `require_website` | string | true or false. Default false |
| `require_phone` | string | true or false. Default false |
| `dedupe` | string | Skip repeats across searches. Default true |
| `language` | string | Two letter language code. Default en |
| `country` | string | Two letter country code for ranking. Default us |
| `max_charge_usd` | string | Optional cost cap for the run |
| `proxy_mode` | string | auto or residential. Default auto |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `place-row`, per business returned | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `apify-actor-start`, per run start | $0.0005 | $0.0005 | $0.0005 | $0.0005 |

Search summary rows, blocked and error rows, filtered businesses, and skipped repeats are free.

## Your keys

The actor needs no key. It reads the public Google Maps search results and holds no Google key or any other key. Proxy traffic adds nothing to your price: Mamba Labs pays the platform costs of the run. `proxy_mode` is a reliability setting.

## Full actor documentation

[apify.com/mambalabs/local-business-lead-builder](https://apify.com/mambalabs/local-business-lead-builder)

## License

MIT. See [LICENSE](LICENSE).
