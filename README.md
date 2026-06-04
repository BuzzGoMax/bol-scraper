[Bol Scraper](https://apify.com/studio-amba/bol-scraper?fpr=data)

# Bol.com Scraper

Extract product data from [Bol.com](https://www.bol.com) — the #1 online retailer in the Netherlands and Belgium — into structured JSON with prices, ratings, specs, stock status, and seller info.

## What is Bol.com Scraper?

**Bol.com Scraper** lets you extract structured e-commerce data from the largest online marketplace in the Benelux region, helping you monitor prices, analyze competitors, and build product databases — all without manual browsing.

- **Track competitor pricing:** extract current and original prices across thousands of products to spot discounts, price drops, and margin opportunities in real time
- **Monitor stock availability:** check which products are in stock, which sellers carry them, and get alerts when inventory changes
- **Build product catalogs:** scrape full product details including specs, images, descriptions, and categories to populate your own catalog or comparison site
- **Analyze market trends:** collect pricing and review data over time to understand which products are gaining popularity or losing traction
- **Feed your analytics pipeline:** export structured data to Google Sheets, Airtable, Power BI, or any tool via Apify integrations

Bol.com has **no public API for product data, no bulk export, and no price history feature**. This scraper is the only way to get structured product data out of the platform at scale.

## What data does Bol.com Scraper extract?

📦 **Product name** and full description
🏷️ **Brand** name
💰 **Price** and original price (discount detection)
💶 **Currency** — always EUR
📊 **Rating** (0-5) and review count
📦 **Stock status** — in stock or out of stock
🔢 **Product ID** and SKU
🖼️ **Images** — main image and all product photos
📋 **Specifications** — key-value pairs (screen size, memory, weight, etc.)
📂 **Category** — full breadcrumb path and category array
🏪 **Seller** — bol.com's own inventory vs. third-party marketplace sellers
🔗 **Direct URL** to the product on Bol.com

## How to scrape Bol.com products

The input is simple: enter a **search keyword** or **category URLs** and hit run. You can configure the scraper through the Apify Console UI or programmatically via the API.

### Input parameters

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `searchQuery` | String | `"laptop"` | Search by keyword — e.g. `"Samsung Galaxy"`, `"LEGO"`, `"AirPods"` |
| `categoryUrls` | Array | — | One or more Bol.com category page URLs to scrape directly |
| `maxResults` | Integer | `100` | Maximum number of products to return (1 - 10,000) |
| `proxyConfiguration` | Object | — | Proxy settings (recommended — Bol.com rate-limits aggressively) |

### Tips for best results

- **Use proxies for any serious volume:** Bol.com rate-limits aggressively. Enable proxy configuration for reliable results beyond a handful of products.
- **Combine search + categories:** use `searchQuery` for discovery and `categoryUrls` to scrape entire product categories systematically
- **Start with a small `maxResults`** (e.g. 50) to verify output, then scale up to thousands
- **Schedule daily runs** to track price changes over time — great for building price history datasets or alerting on discounts

## Output

Results are stored in a **dataset** that you can download in JSON, CSV, Excel, XML, or HTML format directly from the Apify Console.

### JSON example

```
{
    "name": "Samsung Galaxy S24 Ultra 256GB Titanium Black",
    "brand": "Samsung",
    "price": 1299.00,
    "originalPrice": 1449.00,
    "currency": "EUR",
    "productId": "9300000155985953",
    "sku": "0887276789460",
    "inStock": true,
    "rating": 4.6,
    "reviewCount": 142,
    "imageUrl": "https://media.s-bol.com/images/products/samsung-galaxy-s24-ultra-256gb.jpg",
    "imageUrls": [
        "https://media.s-bol.com/images/products/samsung-galaxy-s24-ultra-256gb-1.jpg",
        "https://media.s-bol.com/images/products/samsung-galaxy-s24-ultra-256gb-2.jpg",
        "https://media.s-bol.com/images/products/samsung-galaxy-s24-ultra-256gb-3.jpg"
    ],
    "description": "De Samsung Galaxy S24 Ultra is het vlaggenschip van Samsung met een 6.8 inch Dynamic AMOLED display, 200MP camera en ingebouwde S Pen...",
    "category": "Telefoons > Smartphones",
    "categories": ["Elektronica", "Telefoons", "Smartphones"],
    "specs": {
        "Beeldschermdiagonaal": "6.8 inch",
        "Intern geheugen": "256 GB",
        "Besturingssysteem": "Android 14",
        "Gewicht": "232 g"
    },
    "seller": "bol",
    "url": "https://www.bol.com/nl/nl/p/samsung-galaxy-s24-ultra-256gb-titanium-black/9300000155985953/",
    "scrapedAt": "2026-04-04T14:00:00.000Z"
}
```

## How much does it cost to scrape Bol.com?

Bol.com Scraper uses **only HTTP requests** (no browser), making it fast and affordable to run.

| Scenario | Est. cost | Time |
| --- | --- | --- |
| 100 products | ~$0.15 | ~30 sec |
| 1,000 products | ~$1.50 | ~3 min |
| 10,000 products | ~$15.00 | ~25 min |

**Pricing breakdown:**

- Per run start: $0.01
- Per result: ~$0.0015
- Pay-per-event: $0.004/result

## Can I integrate Bol.com Scraper with other apps?

Yes. Bol.com Scraper connects with any tool through [Apify integrations](https://apify.com/integrations):

- **Google Sheets** — automatically export product data to a spreadsheet
- **Slack / Email** — get notified when prices drop or products go out of stock
- **Zapier / Make** — trigger workflows when data is ready
- **Airtable** — build a searchable product database
- **REST API** — call the scraper programmatically from any language
- **Webhooks** — get notified when a run finishes

## Can I use Bol.com Scraper as an API?

Yes. Use the [Apify API](https://docs.apify.com/api/v2) to run Bol.com Scraper programmatically.

**Python:**

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")
run = client.actor("studio-amba/bol-scraper").call(run_input={
    "searchQuery": "Samsung Galaxy",
    "maxResults": 100,
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"{item['name']} — EUR {item['price']}")
```

**JavaScript:**

```
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: 'YOUR_API_TOKEN' });
const run = await client.actor('studio-amba/bol-scraper').call({
    searchQuery: 'Samsung Galaxy',
    maxResults: 100,
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

Check the [API tab](https://apify.com/studio-amba/bol-scraper/api) for full documentation.

## FAQ

### What is Bol.com?

Bol.com is the largest online retailer in the Netherlands and Belgium, with over 35 million products across electronics, books, toys, fashion, home, garden, and more. It operates both as a direct retailer and as a marketplace where third-party sellers list products — similar to Amazon in the Benelux region.

### How does Bol.com Scraper work?

It parses Bol.com search results and product pages using HTTP requests, extracting structured data from JSON-LD and HTML. No browser automation is needed, which makes it fast, reliable, and cheap to run.

### Can I scrape specific product categories?

Yes. Use the `categoryUrls` input to provide one or more Bol.com category page URLs. The scraper will extract all products from those category pages up to your `maxResults` limit.

### Can I detect price drops and discounts?

Yes. The scraper returns both `price` (current selling price) and `originalPrice` (the strikethrough price shown on Bol.com). Compare these to identify discounted products. Schedule daily runs to build a price history over time.

### Can I tell if a product is sold by Bol.com or a marketplace seller?

Yes. The `seller` field distinguishes between products sold by bol.com directly and those listed by third-party marketplace sellers.

### Does it work for both Netherlands and Belgium?

Yes. Bol.com serves both the Dutch and Belgian markets from a single platform. The scraper uses the `/nl/nl/` path prefix, which covers all products available in both countries.

### Is it legal to scrape Bol.com?

This scraper extracts publicly available product data that Bol.com displays to all visitors. The data is factual (prices, specs, availability) and does not contain private personal information. As with any scraping tool, use the data responsibly and in compliance with applicable laws.

## Limitations

- **Netherlands and Belgium only.** Bol.com operates exclusively in the Benelux market.
- **Proxies recommended.** Bol.com rate-limits aggressively — use proxy configuration for reliable results at scale.
- **Sponsored products included.** Search results may contain promoted/sponsored listings, matching what Bol.com shows to visitors.
- **Dutch product data.** Specs, descriptions, and categories are in Dutch as served by Bol.com.
- **Concurrency capped at 3** to respect Bol.com's rate limits and ensure reliable extraction.

## Other Belgian e-commerce scrapers

Combine Bol.com Scraper with these actors for comprehensive Benelux e-commerce coverage:

- 🛒 [Coolblue Scraper](https://apify.com/studio-amba/coolblue-scraper) — Electronics and appliances from the Benelux's favorite electronics retailer
- 🏪 [Action Scraper](https://apify.com/studio-amba/action-scraper) — Budget products from Europe's fastest-growing discount retailer
- 💊 [Kruidvat Scraper](https://apify.com/studio-amba/kruidvat-scraper) — Health, beauty, and personal care products across Benelux
- 🏃 [Decathlon Scraper](https://apify.com/studio-amba/decathlon-scraper) — Sports and outdoor gear from Belgium's top sports retailer

## Your feedback

Found a bug or have a feature request? Please open an issue on the [Issues tab](https://apify.com/studio-amba/bol-scraper/issues). We actively maintain this scraper and respond to all reports.