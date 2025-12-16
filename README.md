# Google AI Mode Scraper

[![Google AI Mode scraper by cloro](https://github.com/cloro-dev/google-ai-mode-scraper/blob/main/aimode-scraper-hero-image.png)](https://cloro.dev/ai-mode/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Google AI Mode Scraper](https://cloro.dev/ai-mode/) by cloro enables developers to programmatically interact with Google AI Mode and automatically collect AI-powered search responses along with structured metadata. Instead of manual data collection, you can retrieve results as parsed JSON, raw HTML, or other formats for seamless integration into your workflows.

You can use cloro's AI Mode Scraper for general knowledge queries, workflow optimization, and technical guidance. It handles dynamic AI-generated content, supports real-time extraction, and eliminates the need to manage authentication, sessions, or anti-bot systems.

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
| `include.markdown` | Include response in Markdown format when set to true                        | `false`       |
| `include.html`     | Include URL to full HTML response when set to true (URL expires after 48h)  | `false`       |

\* Mandatory parameters

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
        "description": "Comprehensive analysis of emerging AI technologies and their potential impact..."
      },
      {
        "position": 2,
        "url": "https://example.com/machine-learning-report",
        "label": "Tech Industry Report",
        "description": "Industry insights on machine learning adoption and implementation strategies..."
      }
    ],
    "places": [
      {
        "title": "AI Research Institute",
        "rating": 4.7,
        "reviews": 1250,
        "price": "$$$",
        "status": "Open now",
        "address": "123 Tech Street, San Francisco, CA",
        "place_id": "ChIJrTLr-GyuEmsRBfyf1GDs-kY",
        "link": "https://www.google.com/search?q=AI+Research+Institute&kponly&kgmid=ChIJrTLr-GyuEmsRBfyf1GDs-kY",
        "description": "Leading research institute for artificial intelligence and machine learning"
      }
    ],
    "shopping_cards": [
      {
        "title": "AI and Machine Learning Textbook",
        "price": { "value": 89.99, "currency": "$" },
        "store": "Amazon",
        "rating": 4.5,
        "reviews": "1.2k",
        "thumbnail": "https://example.com/book-cover.jpg",
        "product_link": "https://www.google.com/example"
      }
    ],
    "html": "https://storage.cloro.dev/results/c45a5081-808d-4ed3-9c86-e4baf16c8ab8/page-1.html", // URL expires after 48 hours
    "markdown": "**The latest AI and ML trends for 2025** include multimodal AI models, edge computing integration..."
  }
}
```

## Intelligent search capabilities

Google AI Mode provides advanced search capabilities with intelligent understanding and comprehensive responses across a wide range of topics.

### AI Mode features

- **General knowledge**: Comprehensive coverage of topics from science and technology to arts and culture
- **Technical guidance**: Expert-level assistance for technical questions and problem-solving
- **Workflow optimization**: Practical advice for improving processes and productivity
- **Current information**: Access to up-to-date knowledge and recent developments
- **Contextual understanding**: Ability to understand complex queries and provide relevant, detailed responses
- **Location data extraction**: Automatic extraction of places with ratings, reviews, addresses, and operating status
- **Shopping product information**: Structured product data including pricing, store details, ratings, and reviews

### Sources array structure

Each source in the `result.sources` array contains:

| Field         | Type    | Description                                   |
| ------------- | ------- | --------------------------------------------- |
| `position`    | integer | Position order of the source in the response  |
| `url`         | string  | Direct URL to the source content              |
| `label`       | string  | Source name or publication                    |
| `description` | string  | Brief description of what the source contains |

### Places array structure

When location data is present in the AI Mode response, the `result.places` array contains structured place information:

| Field         | Type    | Description                          |
| ------------- | ------- | ------------------------------------ |
| `title`       | string  | Place name                           |
| `rating`      | number  | Star rating (0-5)                    |
| `reviews`     | integer | Number of reviews                    |
| `price`       | string  | Price level (e.g., "$", "$$", "$$$") |
| `status`      | string  | Operating status (e.g., "Open now")  |
| `address`     | string  | Full address                         |
| `thumbnail`   | string  | Place thumbnail image URL            |
| `place_id`    | string  | Google Places ID                     |
| `link`        | string  | Google search URL for the place      |
| `description` | string  | Place description                    |

### Shopping cards array structure

When shopping information is present in the AI Mode response, the `result.shopping_cards` array contains structured product information:

| Field          | Type   | Description                                    |
| -------------- | ------ | ---------------------------------------------- |
| `title`        | string | Product title                                  |
| `price`        | object | Structured pricing with `value` and `currency` |
| `store`        | string | Merchant/store name                            |
| `rating`       | number | Product rating                                 |
| `reviews`      | string | Review count (e.g., "1.2k")                    |
| `thumbnail`    | string | Product image URL                              |
| `product_link` | string | Direct product URL                             |

## Practical AI Mode scraper use cases

1. **General research:** Conduct comprehensive research on any topic with reliable information and sources.
2. **Technical problem-solving:** Get detailed solutions and explanations for technical challenges.
3. **Workflow optimization:** Discover methods to improve business processes and productivity.
4. **Educational content:** Generate educational materials and explanations for complex topics.
5. **Knowledge base creation:** Build comprehensive knowledge bases with sourced information.
6. **Decision support:** Gather well-researched information to support informed decision-making.

## Why choose cloro?

- **Simple integration:** Clean API design with comprehensive documentation and examples.
- **Reliable performance:** >99% uptime and low latencies (P50 < 30s, P90 < 60s)
- **No infrastructure hassle:** We handle rate limiting and browser management.
- **Comprehensive knowledge:** Access to Google AI Mode's broad knowledge base and intelligent search.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping AI Mode allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's AI Mode scraper unique?

cloro's AI Mode endpoint provides reliable access to Google AI Mode's intelligent search with:

- **Comprehensive knowledge coverage** across general and technical topics
- **Structured data extraction** for seamless integration into your workflows
- **Source attribution** for verification and credibility

### What's the recommended timeout for requests?

We recommend setting a timeout of 30-60 seconds for initial requests and 30-45 seconds for retries. Our system handles automatic retries, but implementing your own retry logic with these timeouts provides the best balance between reliability and responsiveness.

### Does the API support different countries?

Yes, you can specify country codes like `US`, `GB`, `DE`, `JP`, `CN`, `IN`, `BR` and more to get localized results relevant to specific regions.

### What kind of questions work best with AI Mode?

AI Mode excels at general knowledge questions, technical inquiries, workflow optimization queries, and any topic requiring comprehensive understanding and detailed explanations.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [docs.cloro.dev](https://docs.cloro.dev)
- **AI Mode scraper page:** [cloro.dev/aimode](https://cloro.dev/ai-mode/)

## Other available scrapers

- **[AI Mode](https://cloro.dev/ai-mode/)** - Extracts structured data from Google AI Mode for general knowledge queries, workflow optimization, and technical guidance.
- **[AI Overview](https://cloro.dev/ai-overview/)** - Extracts structured data from Google AI Overview for comprehensive search result analysis and AI-curated insights.
- **[Gemini](https://cloro.dev/gemini/)** - Extracts structured data from Google Gemini for complex reasoning, content generation, and source confidence scoring.
- **[ChatGPT](https://cloro.dev/chatgpt/)** - Extracts structured data from ChatGPT with advanced features including shopping cards, raw response data, and query fan-out.
- **[Copilot](https://cloro.dev/copilot/)** - Extracts structured data from Microsoft Copilot for development tools, Microsoft ecosystem research, and enterprise-focused queries.
- **[Google](https://cloro.dev/google-search/)** - Extracts structured data from Google Search results, including organic results, People Also Ask questions, related searches, and optional AI Overview data.
- **[Perplexity](https://cloro.dev/perplexity/)** - Extracts comprehensive structured data from Perplexity AI with real-time web sources, automatically detecting and extracting rich data objects.

## Contact us

If you have questions or need support, reach out to us on [our contact page](https://cloro.dev/contact).

---

Built with ❤️ by the cloro team
