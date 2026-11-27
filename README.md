# GLP-1 Price Index (GLP1Prices.co)

[![GLP1Prices.co MCP connector](https://glama.ai/mcp/connectors/co.glp1prices/glp1-price-index/badges/score.svg)](https://glama.ai/mcp/connectors/co.glp1prices/glp1-price-index)

What US providers charge people paying cash for tirzepatide and semaglutide, compounded and brand-name (Zepbound, Mounjaro, Wegovy, Ozempic), read from each provider's own web page and dated. GLP1Prices.co keeps the index current every day and opens it four ways: a remote MCP server for AI assistants, a keyless JSON API, a price list any web page can embed, and a monthly open dataset.

- **Website:** [glp1prices.co](https://glp1prices.co)
- **MCP server:** `https://glp1prices.co/api/mcp` (remote, Streamable HTTP, no key)
- **JSON API:** `https://glp1prices.co/api/v1/prices` ([OpenAPI](https://glp1prices.co/openapi.json), no key, CORS open)
- **Price widget:** `https://glp1prices.co/embed/index` and one list per medicine ([all of them, with the code to paste](https://glp1prices.co/developers#widget))
- **Canadian prices:** [glp1prices.co/canada](https://glp1prices.co/canada), in Canadian dollars, from Canadian online clinics (generic semaglutide, Ozempic®, Wegovy®, Mounjaro®, Zepbound®, Rybelsus®), by medicine and by province; as JSON at `https://glp1prices.co/api/v1/canada?medication=wegovy`
- **RSS feed:** `https://glp1prices.co/feed.xml` (one item each day the index is read, and one per monthly report)
- **Dataset:** November 2026 edition, 1,224 prices from 510 providers: [`glp1prices-november-2026.csv`](glp1prices-november-2026.csv) here, and on Zenodo as [10.5281/zenodo.24523120](https://doi.org/10.5281/zenodo.24523120)
- **Official MCP Registry:** `co.glp1prices/glp1-price-index`

## Ask your assistant

| Tool | What it answers | Try asking |
|------|-----------------|------------|
| `find_glp1_prices` | Today's lowest cash prices of compounded tirzepatide or semaglutide, or of one brand, lowest cost per 30 days first | "Where is compounded tirzepatide cheapest right now?" |
| `find_seller_glp1_prices` | Every current tirzepatide and semaglutide price of one provider, with its plan and any fee billed beside it | "What does Hims & Hers charge for the Wegovy pill?" |
| `find_canada_glp1_prices` | Canadian cash prices of generic semaglutide, Ozempic®, Wegovy®, Mounjaro®, Zepbound® and Rybelsus® at Canadian online clinics, with each clinic's fees and conditions, in C$ | "What does Wegovy cost in Canada without insurance?" |
| `list_glp1_medications` | The price lists the index keeps and how many current prices each holds | "Which GLP-1 medicines does GLP1Prices.co price?" |

All four tools are read-only (`readOnlyHint: true`) and take no account. Every answer links to the page on glp1prices.co that shows each price beside the provider's own words, the page it was read on and the day.

## Connect it

**Claude (web and desktop):** Settings, Connectors, Add custom connector. Name it GLP1Prices.co, paste `https://glp1prices.co/api/mcp`, leave authentication empty.

**Claude Code:**

```bash
claude mcp add --transport http glp1prices https://glp1prices.co/api/mcp
```

**Cursor, Windsurf, Cline, VS Code and other MCP clients:**

```json
{ "mcpServers": { "glp1prices": { "url": "https://glp1prices.co/api/mcp" } } }
```

**ChatGPT:** with developer mode on (Settings, Apps, Advanced settings), create an app with the server URL and no authentication.

## JSON API

| Endpoint | Returns |
|----------|---------|
| `GET /api/v1/prices?medication=tirzepatide` | The lowest prices of one list: `tirzepatide`, `semaglutide` (compounded) or a brand (`zepbound`, `mounjaro`, `wegovy`, `ozempic`); `limit` sets how many |
| `GET /api/v1/sellers?seller=collective` | Every current price of one provider |
| `GET /api/v1/medications` | The lists the index keeps |

Add `source=<your-app>` to any call and the page links in the answer carry your app's name, so visits you send are counted as yours. Each price comes with `cost_per_30_days_usd`, the price as the provider states it, its plan, any fee billed beside it, `checked_on` and `source_url`.

```bash
curl "https://glp1prices.co/api/v1/prices?medication=semaglutide&limit=5&source=my-app"
```

## Show today's prices on your page

Paste one line and your readers see today's lowest prices, updated daily. The list runs no script and sets no cookie; each provider in it links to that provider's prices on GLP1Prices.co, beside its plan, fees and source.

```html
<iframe src="https://glp1prices.co/embed/compounded-tirzepatide?source=your-site&rows=5" title="GLP-1 prices today, from GLP1Prices.co" width="100%" height="420" style="border:0;max-width:520px" loading="lazy"></iframe>
```

Lists: `index` (the index at a glance), `compounded-tirzepatide`, `compounded-semaglutide`, `zepbound`, `wegovy`, `mounjaro`, `ozempic`. Settings: `source` (your site's name, so visits you send count as yours), `rows` (1 to 10), `theme` (`light`, `dark` or `auto`). More at [glp1prices.co/developers](https://glp1prices.co/developers).

## How the prices are read

- Each price is the provider's own, read on its own page. A price stated for a supply of days is put on a 30-day basis; a monthly price on a plan of known length is read as four weekly doses; a yearly membership written into a price is spread over twelve months. A promotional, first-month or coupon price is never taken as a provider's ongoing price. Full method: [glp1prices.co/methodology](https://glp1prices.co/methodology).
- A price not read again within 21 days leaves every list.
- Compounded tirzepatide and semaglutide are made by licensed pharmacies for one patient's prescription and are **not FDA-approved**. A prescription is required for every product listed. Nothing here is medical advice.
- Some providers may pay GLP1Prices.co a referral commission when someone buys through its pages; `may_earn_referral_commission` marks them. It never changes a price, and the lowest-price lists read the price alone.

## Licence

The data is CC BY 4.0 ([LICENSE](LICENSE)). Credit GLP1Prices.co and link the page or report you used, for example: GLP1Prices.co, "GLP-1 Price Index: November 2026," glp1prices.co/reports/november-2026.

## About

GLP1Prices.co is operated by O-mega Enterprise, Inc. Questions: hello@glp1prices.co. This repository documents the hosted server and API and carries the monthly data files; the index itself runs at glp1prices.co.
