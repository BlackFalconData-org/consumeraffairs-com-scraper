# ConsumerAffairs Reviews Scraper

Extract structured data from [consumeraffairs.com](https://consumeraffairs.com) — consumer reviews from consumeraffairs.com for any US company. Export each review with star rating, date and reviewer name plus the company's overall rating as structured data. Incremental mode returns only new or changed reviews on repeat runs.

**[ConsumerAffairs Reviews Scraper on Apify →](https://apify.com/blackfalcondata/consumeraffairs-com-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword. Filter by ↕️ sort reviews by, 📝 description format, and more.

**Detail enrichment** — Fetch employer profiles, structured metadata for each record.

**Incremental mode** — Only get new or changed records since your last run. Content hash per record — no duplicates, no re-processing.

**Change classification** — Track cross-run duplicate detection across runs. Build audit trails of how records evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N records (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every record returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured records from consumeraffairs.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor records, track trends, and analyze market dynamics with structured, deduplicated data from consumeraffairs.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per record is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "Verizon Wireless",
  "maxResults": 100,
  "reviewTextMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
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
| `query` | string | — | Company name as listed on ConsumerAffairs (e.g. "Verizon Wireless"). Use a JSON array for several companies: ["Verizon Wireless","T-Mobile"]. Required if no start URLs are provided. |
| `maxResults` | integer | `100` | Maximum total reviews (0 = every listed review). |
| `startUrls` | array | `[]` | Paste ConsumerAffairs company profile URLs (e.g. https://www.consumeraffairs.com/cell_phones/verizon_wireless.html). Each URL becomes its own task; reviews are merged and deduped by review ID across all URLs. When provided, start URLs REPLACE the company field. |
| `maxReviewsPerCompany` | integer | `0` | Stop after this many reviews for each company, so one large company does not use up Max Reviews for the rest. 0 = no per-company limit. |
| `reviewsSince` | string | — | Only reviews dated on or after this day (YYYY-MM-DD). When sorted newest first, reading stops at the first page of older reviews. Leave empty for every review. |
| `sortBy` | enum | `"recent"` | Order the reviews are read in: newest first, oldest first, most helpful, or the site's top reviews. Max Reviews keeps the first ones in this order. |
| `starRatings` | array | `[]` | Only reviews with these star ratings, e.g. 1 and 2 for complaints. Leave empty for every rating. |
| `reviewTextMaxLength` | integer | `0` | Truncate the review body to N characters. 0 = full text. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against the previous run and return only new or changed reviews. stateKey is optional — defaults to a value derived from the companies / start URLs you asked for, so different inputs never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked set of reviews. Leave empty to auto-generate from the company / start URLs. |
| `skipReposts` | boolean | `false` | When incremental, skip reviews whose content matches a review that disappeared in a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED reviews. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of reviews included in each notification message (1–20). |
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
| `includeDetails` | boolean | `false` | Fetch the detail page for each listing to retrieve the full description text and contact information. Increases run time and cost. Leave off for a fast, low-cost run. |

---

## Output fields

Every record returns the same 41-field schema. Missing values are `null` — never omitted.

- `listingId`
- `reviewId`
- `title`
- `entityName`
- `location`
- `url`
- `portalUrl`
- `rating`
- `reviewDate`
- `reviewDateText`
- `reviewUpdated`
- `originalReviewDate`
- `verifiedPurchase`
- `helpfulCount`
- `tags`
- `text`
- `images`
- `authorName`
- `authorLocation`
- `authorAvatar`
- `companyResponse`
- `companyResponseDate`
- `ratingUpdateNote`
- `companyName`
- `companyUrl`
- `companyRating`
- `companyRatingCount`
- `companyCategory`
- `companyCategoryUrl`
- `companyLogo`
- `companyPhone`
- `companyAccredited`
- `page`
- `position`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per record. Here is a real example from a production run:

```json
{
  "listingId": "5060bd866061847de68df76385d061e5fa871ad6b01991cc8b05a3696b5f3b3c",
  "reviewId": "14072832",
  "title": "Verizon Wireless",
  "entityName": "Verizon Wireless",
  "location": "Valparaiso, IN",
  "url": "https://www.consumeraffairs.com/cell_phones/verizon_wireless.html#review=14072832",
  "portalUrl": "https://www.consumeraffairs.com/cell_phones/verizon_wireless.html#review=14072832",
  "rating": 1,
  "reviewDate": "2026-10-13",
  "reviewDateText": "Reviewed Oct. 13, 2026",
  "reviewUpdated": false,
  "originalReviewDate": null
}
```

*Truncated — full records contain 41 fields. See Output fields for the complete schema.*

**[Try ConsumerAffairs Reviews Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/consumeraffairs-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.004473 |

See the [actor on Apify](https://apify.com/blackfalcondata/consumeraffairs-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape consumeraffairs.com?**
Use this actor on Apify to extract structured data from consumeraffairs.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get consumeraffairs.com data as JSON, CSV, or Excel?**
The actor writes each record to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape consumeraffairs.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check consumeraffairs.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each record gets a content hash. On subsequent runs, only new or changed records are emitted — saving time, compute, and storage. Expired records can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [Djinni Scraper](https://apify.com/blackfalcondata/djinni-scraper?fpr=1h3gvi) — Ukraine & CEE tech jobs with salary and English level
- [Work.ua Scraper](https://apify.com/blackfalcondata/work-ua-scraper?fpr=1h3gvi) — Ukraine's work.ua job listings
- [NoFluffJobs Scraper](https://apify.com/blackfalcondata/nofluffjobs-scraper?fpr=1h3gvi) — Poland & CEE IT jobs with transparent salary ranges
- [OLX Jobs Scraper](https://apify.com/blackfalcondata/olx-jobs-scraper?fpr=1h3gvi) — OLX job listings across 7 CEE countries
- [Prace.cz Scraper](https://apify.com/blackfalcondata/prace-scraper?fpr=1h3gvi) — Czech Republic's prace.cz job board
- [Profession.hu Scraper](https://apify.com/blackfalcondata/profession-scraper?fpr=1h3gvi) — Hungary's profession.hu job board

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [ConsumerAffairs Reviews Scraper](https://apify.com/blackfalcondata/consumeraffairs-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
