# Google AI Mode Scraper

[![Google AI Mode scraper by cloro](https://github.com/cloro-dev/google-ai-mode-scraper/blob/main/aimode-scraper-hero-image.png)](https://cloro.dev/ai-mode/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Google AI Mode Scraper](https://cloro.dev/ai-mode/) by cloro lets developers programmatically interact with Google AI Mode and collect AI search responses with structured metadata. You can retrieve results as parsed JSON, raw HTML, or other formats for integration into your workflows.

You can use cloro's AI Mode Scraper for general knowledge queries, workflow optimization, and technical guidance. It handles dynamic AI-generated content, supports real-time extraction, and removes the need to manage authentication, sessions, or anti-bot systems.

## How it works

The AI Mode scraper handles the rendering, parsing, and delivery of results in your requested format. You provide your search query, API credentials, and optional parameters as shown below.

### Request sample (Python)

```python
import json
import requests

# API parameters
payload = {
    'prompt': 'What are the latest trends in artificial intelligence and machine learning for 2025?',
    'country': 'US',
    'include': {
        'markdown': True
    }
}

# Get a response
response = requests.post(
    'https://api.cloro.dev/v1/monitor/aimode',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload
)

# Print response to stdout
print(response.json())

# Save response to a JSON file
with open('response.json', 'w') as file:
    json.dump(response.json(), file, indent=2)
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/aimode \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "What are the latest trends in artificial intelligence and machine learning for 2025?",
    "country": "US",
    "include": {
      "markdown": true
    }
  }'
```

### Request sample (Node.js)

```javascript
const axios = require("axios");

const payload = {
  prompt:
    "What are the latest trends in artificial intelligence and machine learning for 2025?",
  country: "US",
  include: {
    markdown: true,
  },
};

axios
  .post("https://api.cloro.dev/v1/monitor/aimode", payload, {
    headers: {
      Authorization: "Bearer YOUR_API_KEY",
      "Content-Type": "application/json",
    },
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

### Request parameters

| Parameter          | Description                                                                 | Default value |
| ------------------ | --------------------------------------------------------------------------- | ------------- |
| `prompt`\*         | The search query or question (1-10,000 characters)                          | –             |
| `country`          | Optional country/region code for localized results (e.g., `US`, `GB`, `DE`) | `US`          |
| `location`         | [Google canonical location name](https://developers.google.com/google-ads/api/reference/data/geotargets) for geo-targeted results (e.g., `New York,New York,United States`). Mutually exclusive with `uule` | –             |
| `uule`             | Pre-encoded Google UULE string for precise geo-targeting. Mutually exclusive with `location` | –             |
| `device`           | Device type for search results (`desktop` or `mobile`)                      | `desktop`     |
| `include.markdown` | Include response in Markdown format when set to true                        | `false`       |
| `include.html`     | Include URL to full HTML response when set to true (URL expires after 24h)  | `false`       |

\* Mandatory parameters

### Geo-targeted request sample (Python)

```python
import json
import requests

# API parameters with city-level geo-targeting
payload = {
    'prompt': 'best restaurants nearby',
    'country': 'US',
    'location': 'New York,New York,United States',
    'include': {
        'markdown': True
    }
}

# Get a response
response = requests.post(
    'https://api.cloro.dev/v1/monitor/aimode',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload
)

# Print response to stdout
print(response.json())
```

---

### Output samples

The AI Mode Scraper API returns a structured JSON object containing AI Mode's intelligent search response and metadata.

**Structured JSON output snippet:**

```json
{
  "success": true,
  "result": {
    "text": "The latest AI and ML trends for 2025 include multimodal AI models, edge computing integration, ethical AI frameworks, and generative AI advancements...",
    "sources": [
      {
        "position": 1,
        "url": "https://example.com/ai-trends-2025",
        "label": "AI Research Institute",
        "description": "Analysis of emerging AI technologies and their potential impact..."
      },
      {
        "position": 2,
        "url": "https://example.com/machine-learning-report",
        "label": "Tech Industry Report",
        "description": "Industry insights on machine learning adoption and implementation strategies..."
      }
    ],
    "map": [
      {
        "title": "AI Research Institute",
        "link": "https://www.google.com/viewer/place?mid=/g/11bw3yq9kp",
        "placeId": "/g/11bw3yq9kp",
        "index": 0,
        "gps_coordinates": { "latitude": 37.7749, "longitude": -122.4194 },
        "thumbnail": "https://lh5.googleusercontent.com/p/example",
        "rating": 4.7,
        "reviews": 1250,
        "type": "Research institute",
        "address": "123 Tech Street, San Francisco, CA",
        "status": "Open now"
      }
    ],
    "places": [
      {
        "title": "Gion District",
        "link": "https://www.google.com/viewer/place?mid=/g/122m3x9s",
        "placeId": "/g/122m3x9s",
        "index": 0,
        "thumbnail": "https://lh5.googleusercontent.com/p/example2",
        "rating": 4.6,
        "reviews": 2900,
        "type": "Tourist attraction",
        "priceLevel": "$$",
        "address": "Gionmachi, Higashiyama Ward, Kyoto",
        "status": "Open now"
      }
    ],
    "shoppingCards": [
      {
        "title": "AI and Machine Learning Textbook",
        "price": { "value": 89.99, "currency": "$" },
        "oldPrice": { "value": 129.99, "currency": "$" },
        "store": "Amazon",
        "rating": 4.5,
        "reviews": "1.2k",
        "thumbnail": "https://example.com/book-cover.jpg",
        "productLink": "https://www.google.com/example",
        "snippet": "Guide to modern AI and machine learning techniques",
        "snippet_links": [{ "text": "machine learning", "link": "https://www.google.com/search?q=machine+learning" }]
      }
    ],
    "ads": {
      "title": "AI books to consider",
      "ads": [
        {
          "title": "Deep Learning Fundamentals",
          "url": "https://www.google.com/aclk?sa=L&ai=abc",
          "position": 1,
          "price": { "value": 49.99, "currency": "$" },
          "store": "O'Reilly Media",
          "rating": 4.8,
          "reviews": "2.1k"
        }
      ]
    },
    "inlineProducts": [
      {
        "title": "Neural Network Workstation",
        "price": { "value": 2499.00, "currency": "$" },
        "oldPrice": { "value": 2999.00, "currency": "$" },
        "store": "NVIDIA",
        "thumbnail": "https://example.com/workstation.jpg",
        "productLink": "https://www.google.com/shopping/product/example"
      }
    ],
    "html": "https://storage.cloro.dev/results/c45a5081-808d-4ed3-9c86-e4baf16c8ab8/page-1.html", // URL expires after 24 hours
    "markdown": "**The latest AI and ML trends for 2025** include multimodal AI models, edge computing integration...[AI Research Institute](https://example.com/ai-trends-2025)[Tech Industry Report](https://example.com/machine-learning-report)"
  }
}
```

## Intelligent search capabilities

Google AI Mode provides search capabilities with intelligent understanding and detailed responses across a range of topics.

### AI Mode features

- **General knowledge**: Coverage of topics from science and technology to arts and culture
- **Technical guidance**: Assistance for technical questions and problem-solving
- **Workflow optimization**: Practical advice for improving processes and productivity
- **Current information**: Access to up-to-date knowledge and recent developments
- **Contextual understanding**: Handling of complex queries with relevant, detailed responses
- **Map data extraction**: GPS-enriched location results with coordinates, ratings, reviews, and operating status
- **Place card extraction**: Inline place cards with ratings, reviews, price levels, and operating status
- **Shopping product information**: Structured product data including pricing, discount prices, store details, ratings, reviews, and product snippets
- **Ads extraction**: Automatic parsing of sponsored ad sections with product details, pricing, and store information
- **Inline product extraction**: Product cards embedded within the AI response text, extracted separately from shopping carousels

### Sources array structure

Each source in the `result.sources` array contains:

| Field         | Type    | Description                                   |
| ------------- | ------- | --------------------------------------------- |
| `position`    | integer | Position order of the source in the response  |
| `url`         | string  | Direct URL to the source content              |
| `label`       | string  | Source name or publication                    |
| `description` | string  | Brief description of what the source contains |

### Citation pills array structure

When the AI Mode answer carries pill chips, the `result.citationPills` array exposes each cited source as a self-contained entry. When a pill cites N sources, the array contains N entries sharing the same `citationPillId` but with per-source `label`, `url`, and `domain`. Group by `citationPillId` to recover pill-level structure. The field is omitted when no pills are present.

| Field            | Type    | Description                                                                                                                                          |
| ---------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `label`          | string  | Per-source title from the sources rail (e.g. `"AI Research Institute"`). Always present; may be an empty string when the rail has no title for this source — read `domain` / `url` for source identity in that case. |
| `citationPillId` | integer | Stable identifier shared by all entries from the same visible chip. 1-based ordinal in document order.                                               |
| `url`            | string  | Direct URL of the cited source.                                                                                                                      |
| `domain`         | string  | Host extracted from `url`, for grouping and display.                                                                                                 |
| `description`    | string  | Source snippet from the sources rail when Google ships one. Omitted when absent.                                                                     |
| `position`       | integer | 1-based position of this source in the sibling `result.sources` array.                                                                               |

### Map array structure

When GPS-enriched location data is present, the `result.map` array contains structured map entries with coordinates and contact details:

| Field             | Type    | Description                                        |
| ----------------- | ------- | -------------------------------------------------- |
| `title`           | string  | Place name                                         |
| `link`            | string  | Google viewer URL for the place                    |
| `placeId`        | string  | Google Places ID                                   |
| `index`           | integer | Position index in the results                      |
| `gps_coordinates` | object  | Geographic coordinates (`latitude`, `longitude`)   |
| `thumbnail`       | string  | Place thumbnail image URL                          |
| `rating`          | number  | Star rating (0-5)                                  |
| `reviews`         | integer | Number of reviews                                  |
| `type`            | string  | Place type or category (e.g., "Coffee shop")       |
| `address`         | string  | Full address                                       |
| `status`          | string  | Operating status (e.g., "Open now")                |

### Places array structure

When inline place cards are present, the `result.places` array contains place information without GPS coordinates. These are distinct from map results.

| Field         | Type    | Description                                    |
| ------------- | ------- | ---------------------------------------------- |
| `title`       | string  | Place name                                     |
| `link`        | string  | Google viewer URL for the place                |
| `placeId`    | string  | Google Places ID                               |
| `index`       | integer | Position index in the results                  |
| `thumbnail`   | string  | Place thumbnail image URL                      |
| `rating`      | number  | Star rating (0-5)                              |
| `reviews`     | integer | Number of reviews                              |
| `type`        | string  | Place type or category                         |
| `priceLevel` | string  | Price level indicator (e.g., "$", "$$", "$$$") |
| `address`     | string  | Full address                                   |
| `status`      | string  | Operating status (e.g., "Open now")            |

### Shopping cards array structure

When shopping information is present in the AI Mode response, the `result.shoppingCards` array contains structured product information:

| Field           | Type   | Description                                    |
| --------------- | ------ | ---------------------------------------------- |
| `title`         | string | Product title                                  |
| `price`         | object | Structured pricing with `value` and `currency` |
| `oldPrice`     | object | Original price before discount                 |
| `store`         | string | Merchant/store name                            |
| `rating`        | number | Product rating                                 |
| `reviews`       | string | Review count (e.g., "1.2k")                    |
| `thumbnail`     | string | Product image URL                              |
| `productLink`  | string | Direct product URL                             |
| `snippet`       | string | Product description snippet                    |
| `snippet_links` | array  | Links within the snippet (`text`, `link`)      |

### Ads object structure

When sponsored results are present, the `result.ads` object contains a section title and an array of ads:

| Field      | Type    | Description                                    |
| ---------- | ------- | ---------------------------------------------- |
| `title`    | string  | Section title for the ads                      |
| `ads`      | array   | Array of sponsored ads (see below)             |

Each ad in the `ads` array:

| Field      | Type    | Description                                    |
| ---------- | ------- | ---------------------------------------------- |
| `title`    | string  | Ad product title                               |
| `url`      | string  | Ad click-through URL                           |
| `position` | integer | Position index of the ad                       |
| `price`    | object  | Structured pricing with `value` and `currency` |
| `store`    | string  | Merchant/store name                            |
| `rating`   | number  | Product rating                                 |
| `reviews`  | string  | Review count                                   |

### Inline products array structure

When product cards are embedded within the AI response text, the `result.inlineProducts` array contains structured product information. These are distinct from shopping cards, which appear in dedicated carousels.

| Field          | Type   | Description                                    |
| -------------- | ------ | ---------------------------------------------- |
| `title`        | string | Product title                                  |
| `price`        | object | Structured pricing with `value` and `currency` |
| `oldPrice`    | object | Original price before discount                 |
| `store`        | string | Merchant/store name                            |
| `thumbnail`    | string | Product image URL                              |
| `productLink` | string | Direct product URL                             |

## Practical AI Mode scraper use cases

1. **General research:** Conduct research on any topic with sources.
2. **Technical problem-solving:** Get solutions and explanations for technical challenges.
3. **Workflow optimization:** Discover methods to improve business processes and productivity.
4. **Educational content:** Generate educational materials and explanations.
5. **Knowledge base creation:** Build knowledge bases with sourced information.
6. **Decision support:** Gather information to support decision-making.

## Why choose cloro?

- **Simple integration:** Clean API design with documentation and examples.
- **Reliable performance:** >99% uptime and low latencies (P50 < 30s, P90 < 60s)
- **No infrastructure hassle:** We handle rate limiting and browser management.
- **Knowledge access:** Access to Google AI Mode's knowledge base and intelligent search.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping AI Mode allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's AI Mode scraper unique?

cloro's AI Mode endpoint provides access to Google AI Mode's intelligent search with:

- **Knowledge coverage** across general and technical topics
- **Structured data extraction** for direct integration into your workflows
- **Source attribution** for verification and credibility

### What's the recommended timeout for requests?

We don't recommend putting any timeout, given that our system retries automatically. We recommend setting up a retry mechanism in case of failure.

### Does the API support different countries?

Yes, you can specify country codes like `US`, `GB`, `DE`, `JP`, `CN`, `IN`, `BR` and more to get localized results relevant to specific regions.

### What kind of questions work best with AI Mode?

AI Mode handles general knowledge questions, technical inquiries, workflow optimization queries, and topics requiring detailed explanations.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [cloro.dev/docs](https://cloro.dev/docs/)
- **AI Mode scraper page:** [cloro.dev/ai-mode](https://cloro.dev/ai-mode/)

## Other available scrapers

- **[AI Mode](https://cloro.dev/ai-mode/)** - Extracts structured data from Google AI Mode for general knowledge queries, workflow optimization, and technical guidance.
- **[AI Overview](https://cloro.dev/ai-overview/)** - Extracts structured data from Google AI Overview for comprehensive search result analysis and AI-curated insights.
- **[ChatGPT](https://cloro.dev/chatgpt/)** - Extracts structured data from ChatGPT with advanced features including shopping cards, raw response data, and query fan-out.
- **[Copilot](https://cloro.dev/copilot/)** - Extracts structured data from Microsoft Copilot for development tools, Microsoft ecosystem research, and enterprise-focused queries.
- **[Gemini](https://cloro.dev/gemini/)** - Extracts structured data from Google Gemini for complex reasoning, content generation, and source confidence scoring.
- **[Google Search](https://cloro.dev/google-search/)** - Extracts structured data from Google Search results, including organic results, People Also Ask questions, related searches, and optional AI Overview data.
- **[Google News](https://cloro.dev/google-news/)** - Extracts structured news articles from Google News with titles, snippets, sources, dates, and thumbnail images for news monitoring and media tracking.
- **[Grok](https://cloro.dev/grok/)** - Extracts structured data from Grok for current events, news tracking, and real-time information gathering.
- **[Perplexity](https://cloro.dev/perplexity/)** - Extracts comprehensive structured data from Perplexity AI with real-time web sources, automatically detecting and extracting rich data objects.

## Contact us

If you have questions or need support, reach out to us at [support@cloro.dev](mailto:support@cloro.dev).

---

Built with ❤️ by the cloro team
