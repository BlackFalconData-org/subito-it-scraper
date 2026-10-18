# Subito Italy Scraper [Just 💰$0.45]

Extract structured data from [subito.it](https://subito.it) — subito.it, Italy's classifieds marketplace: price, condition, category, location with coordinates, photos, seller and full ad text for every listing. Search by keyword, region or category URL, then filter by radius around a point and track changes between runs.

**[Subito Italy Scraper [Just 💰$0.45] on Apify →](https://apify.com/blackfalcondata/subito-it-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from subito.it on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from subito.it.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "iphone 15",
  "maxResults": 100,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all",
  "includeDetails": false
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Keywords to search subito.it for, in Italian (e.g. "iphone 15", "bici da corsa", "appartamento"). Paste a JSON array like ["iphone 15","ipad"] to run several searches in one go. Required if no startUrls provided. |
| `location` | string | — | Optional subito.it region filter, given as the region id (e.g. 4 = Lombardia, 6 = Veneto, 8 = Emilia-Romagna, 11 = Lazio, 20 = Sicilia). Leave empty to search all of Italy. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste subito.it search-results URLs, for example https://www.subito.it/annunci-lombardia/vendita/telefonia/?q=iphone. The keyword (q), the region (annunci-lombardia) and the category (telefonia) in the URL are applied; other filters on the page are not. Province and town pages and single ad pages are not supported. Each URL becomes its own search and results are deduplicated by listing across all URLs. When provided, startUrls replace the query and region fields; unsupported URLs are skipped, and if none is supported the run stops with an input error. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `includeDetails` | boolean | `false` | Every result already includes the full description, photos, location, price and seller name, so this switch changes nothing in the output. It is kept so existing inputs keep working. |
| `fetchSellerProfile` | boolean | `false` | Add seller reputation to every result: seller_rating, seller_review_count, seller_member_since, seller_verified, seller_last_activity, seller_reply_time_minutes, seller_badges, plus the seller profile link and, for business sellers, the shop id, name and link. One lookup per distinct seller. Off by default. |
| `fetchSellerPhone` | boolean | `false` | Add the seller phone number (seller_phone) to every result where the seller shows one on the ad - mostly business sellers; the field is null otherwise. One lookup per result. Off by default. |
| `enrichEmails` | boolean | `true` | Add an emails list to every result with the e-mail addresses found in the ad description (lowercased, deduplicated; empty when the text has none). Free: no extra lookups. Turn off to leave the field null. |
| `lat` | number | — | Latitude of the centre point, between -90 and 90. Use together with lon and Search Radius: results are limited to listings whose town lies within the radius of this point. Each result then carries distanceKm. |
| `lon` | number | — | Longitude of the centre point, between -180 and 180. Use together with lat and Search Radius. |
| `radiusKm` | integer | — | Only return listings whose town lies within this many kilometres of the latitude/longitude point. Listings with no town coordinates are kept, with distanceKm empty. Leave empty (or 0) for no distance limit. Max Results counts the listings that pass the filter. |

---

## Output fields

Every product returns the same 58-field schema. Missing values are `null` — never omitted.

- `urn`
- `ad_id`
- `list_id`
- `title`
- `description`
- `ad_type`
- `ad_type_key`
- `category`
- `category_id`
- `category_slug`
- `macrocategory_id`
- `posted_at`
- `expires_at`
- `image_count`
- `image_urls`
- `region`
- `region_id`
- `city`
- `city_id`
- `city_short`
- `town`
- `town_id`
- `lat`
- `lon`
- `distanceKm`
- `geo`
- `advertiser`
- `seller_profile_url`
- `seller_rating`
- `seller_review_count`
- `seller_member_since`
- `seller_verified`
- `seller_last_activity`
- `seller_reply_time_minutes`
- `seller_badges`
- `seller_shop_id`
- `seller_shop_name`
- `seller_shop_url`
- `seller_phone`
- `emails`
- `url`
- `portalUrl`
- `mobile_url`
- `features`
- `price`
- `price_label`
- `currency`
- `shipping_cost`
- `condition`
- `shippable`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "urn": "id:ad:612896528:list:664402064",
  "ad_id": "612896528",
  "list_id": "664402064",
  "title": "Airbag volante vw beetle 2012 5c5880201b 5c5880201",
  "description": "AIRBAG VOLANTE VW BEETLE 2012 5C5880201B 5C5880201AE WA3\n\nNote del venditore : NON SEI SICURO? CONTATTACI...\nTipo : Moduli airbag\nCodice ricambio originale OE/OEM : 5C5880201B, 5C5…",
  "ad_type": "In vendita",
  "ad_type_key": "s",
  "category": "Accessori Auto",
  "category_id": "5",
  "category_slug": "accessori-auto",
  "macrocategory_id": "1",
  "posted_at": "2026-10-13T16:01:52.37+0200"
}
```

*Truncated — full records contain 58 fields. See Output fields for the complete schema.*

**[Try Subito Italy Scraper [Just 💰$0.45] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/subito-it-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.009 |
| Result | $0.00045 |

See the [actor on Apify](https://apify.com/blackfalcondata/subito-it-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape subito.it?**
Use this actor on Apify to extract structured data from subito.it. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get subito.it data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape subito.it?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check subito.it's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Subito Italy Scraper [Just 💰$0.45]](https://apify.com/blackfalcondata/subito-it-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
