# Google AI Mode Scraper

[![Google AI Mode scraper by cloro](https://github.com/cloro-dev/google-ai-mode-scraper/blob/main/aimode-scraper-hero-image.png)](https://cloro.dev/ai-mode/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Google AI Mode scraper](https://cloro.dev/ai-mode/?utm_source=github) by cloro returns AI Mode answers as structured JSON: the full text and markdown, cited sources, citation pills, place cards, shopping data and ads. One POST request, no browser to run.

## How do you scrape Google AI Mode?

1. Get an API key at [cloro.dev](https://cloro.dev/?utm_source=github&utm_medium=readme).
2. POST a prompt to `https://api.cloro.dev/v1/monitor/aimode`.
3. Read `result.text`, `result.sources[]` and `result.citationPills[]` from the JSON.

cloro renders the AI Mode interface, waits for the streamed answer to finish, parses the citation metadata Google hides in HTML comments, and returns JSON. CAPTCHAs, proxy rotation, session state and selector drift are handled server-side.

The hard part, if you build it yourself, is that AI Mode puts no source URLs in anchor tags. Each source is a button tied by UUID to an HTML comment, and nothing is in the initial page HTML because the answer streams in over a separate call.

### Request sample (Python)

```python
import json
import requests

payload = {
    'prompt': 'What are the latest trends in artificial intelligence and machine learning?',
    'country': 'US',
    'include': {'markdown': True},
}

response = requests.post(
    'https://api.cloro.dev/v1/monitor/aimode',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload,
)

print(response.json())
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/aimode \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": "best coffee shops in downtown Austin", "country": "US"}'
```

Node.js and async/webhook examples are in the [endpoint documentation](https://cloro.dev/docs/api-reference/endpoint/monitor-aimode).

### Request parameters

| Parameter | Description | Default |
| --- | --- | --- |
| `prompt`\* | The search query or question (1-10,000 characters) | – |
| `country` | Country code for localized results (`US`, `GB`, `DE`) | `US` |
| `location` | [Google canonical location name](https://developers.google.com/google-ads/api/reference/data/geotargets) for geo-targeting. Mutually exclusive with `uule` | – |
| `uule` | Pre-encoded Google UULE string. Mutually exclusive with `location` | – |
| `device` | `desktop` or `mobile` | `desktop` |
| `include.markdown` | Return the answer as Markdown | `false` |
| `include.html` | Return a URL to the full HTML (expires after 24h) | `false` |

\* Required

## What data does the AI Mode scraper return?

```json
{
  "success": true,
  "result": {
    "text": "The latest AI and ML trends include multimodal AI models, edge computing integration...",
    "sources": [
      {
        "position": 1,
        "url": "https://example.com/ai-trends",
        "label": "AI Research Institute",
        "description": "Analysis of emerging AI technologies..."
      }
    ],
    "citationPills": [{ "citationPillId": "a1b2", "label": "AI Research Institute", "url": "https://example.com/ai-trends", "domain": "example.com" }],
    "places": [{ "title": "Gion District", "rating": 4.6, "reviews": 2900, "type": "Tourist attraction", "priceLevel": "$$", "status": "Open now" }],
    "shoppingCards": [{ "title": "AI and Machine Learning Textbook", "price": { "value": 89.99, "currency": "$" }, "store": "Amazon", "rating": 4.5 }],
    "markdown": "**The latest AI and ML trends** include multimodal AI models...[AI Research Institute](https://example.com/ai-trends)"
  }
}
```

Seven arrays come back alongside `text` and `markdown`:

1. **`sources`** — every cited URL with position, label and description.
2. **`citationPills`** — the inline pill chips. One entry per source, sharing a `citationPillId` when a pill cites several; group by that id to recover pill structure.
3. **`map`** — GPS-enriched map results with coordinates, rating, reviews, address and operating status.
4. **`places`** — inline place cards with rating, reviews, price level and status.
5. **`shoppingCards`** — product carousels with price, old price, store, rating and reviews.
6. **`ads`** — sponsored sections parsed into product, pricing and store fields.
7. **`inlineProducts`** — product cards embedded in the answer text, separate from the carousels.

Full field-level schemas: [citation pills](https://cloro.dev/docs/api-reference/endpoint/aimode/citation-pills), [sources](https://cloro.dev/docs/api-reference/endpoint/aimode/sources), and the [endpoint reference](https://cloro.dev/docs/api-reference/endpoint/monitor-aimode).

## Use cases

- **AI brand monitoring** — track whether AI Mode names your brand and which sources it cites.
- **Local SEO** — read place cards for a specific city using `location` or `uule`.
- **Shopping and price tracking** — pull the product carousels and inline product cards.
- **Citation research** — study which domains AI Mode reaches for on a query class.

## FAQ

### Is scraping Google AI Mode allowed?

cloro returns publicly visible search results, the same page a signed-out user sees. No court has ruled that scraping public results pages is unlawful. Check your own jurisdiction and use case, and do not use the output to reproduce copyrighted content.

### How is AI Mode different from AI Overview?

They are different content systems on the same SERP. Measured across 1.3 million AI Mode citations, the two cite the same URLs only 13.7% of the time, so scraping one tells you little about the other. See the [AI Overview scraper](https://cloro.dev/ai-overview/) for that surface.

### What is the recommended timeout?

60 seconds. AI Mode streams its answer, so a response can take several seconds longer than a standard SERP fetch. Use the async endpoint for batch workloads.

### Does the API support geo-targeting?

Yes. `country` for national results, or `location` and `uule` for city-level precision. Place-card results differ significantly between a national average and a specific metro.

## Learn more

- **Endpoint reference:** [cloro.dev/docs](https://cloro.dev/docs/api-reference/endpoint/monitor-aimode)
- **Product page:** [cloro.dev/ai-mode](https://cloro.dev/ai-mode/)

## Other cloro scrapers

[AI Overview](https://cloro.dev/ai-overview/) · [ChatGPT](https://cloro.dev/chatgpt/) · [Copilot](https://cloro.dev/copilot/) · [Gemini](https://cloro.dev/gemini/) · [Google Search](https://cloro.dev/google-search/) · [Google News](https://cloro.dev/google-news/) · [Grok](https://cloro.dev/grok/) · [Perplexity](https://cloro.dev/perplexity/)

## Contact us

Questions or support: [r/cloroapi](https://www.reddit.com/r/cloroapi/).
