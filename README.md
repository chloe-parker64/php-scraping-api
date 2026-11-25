# PHP Scraping API How to Choose a Cheaper Geekflare Plan for High-Volume Data Pulls — Detailed Cost Breakdown Shows If It Is Worth It for Your eCommerce Operations and Real-Time Price Tracking?

When you spin up a PHP service that needs to pull product listings, price updates, or user-generated content from the wild, the first question that pops up is *"how much will this cost me?"*. The answer is rarely a simple dollar-per-request number; most providers hide multipliers, hidden proxy fees, or tier-specific rate limits that turn a modest workload into a surprise invoice. This guide walks through every detail you need to know about the **Geekflare Web Scraping API**—the PHP-friendly, credit-based service that bundles headless Chrome rendering, CAPTCHA solving, and anti-bot bypass into a single credit. We'll compare Geekflare side-by-side with three well-known competitors, break down per-1,000-request economics, show you exactly how to call the endpoint from PHP, and help you pick the right plan (or credit pack) for your real-world traffic patterns. All numbers are taken from official Geekflare documentation; no guesswork, no hidden math.

---

## An Independent Side-by-Side Look at Leading Providers

| Provider | Free Tier? | Base Credit Cost (Standard) | Proxy Add-On | AI Extraction | Rate Limits (req/s) | Pricing Details |
|----------|------------|----------------------------|-------------|---------------|----------------------|-----------------|
| ScraperAPI | Yes (limited) | Credit multipliers up to 75× for JS & geo | Included in higher multipliers | Not a separate add-on | 5 / 10 / 20 (req/s) | See ScraperAPI pricing |
| Scrapingdog | Yes (30 K req/mo) | $0.45 / 1 K JS (standard) | 10 credits base, up to 25 credits with JS | Not a separate add-on | 10 (req/s) | See Scrapingdog pricing |
| **Geekflare** | Yes (500 credits/mo) | 1 credit per request (≈ $0.0019 / 1 K on Starter) | +4 credits on paid plans only | +7 credits (total 8) on paid plans | 1 / 5 / 10 / 25 (req/s) (Enterprise up to 1,000) | [Pricing](https://geekflare.com/pricing/) |
| Zyte | No free tier | Dynamic per-URL pricing (depends on target) | Built-in proxy pool | Optional extraction add-on | 5 / 10 (req/s) | See Zyte pricing |

**Takeaway:** Geekflare's biggest advantage is transparency—every request consumes a fixed number of credits, and the credit price is printed on each plan page. Competitors rely on opaque multipliers that can explode costs when you need JavaScript rendering or geo-targeting. Proxy usage adds **+4 credits per request on paid plans only** (not available on the Free tier). If you need predictable budgeting for a PHP scraper, Geekflare stays the cleanest choice.

---

## Real Per-1,000-Request Economics Explained Plainly

Below we translate Geekflare's credit pricing into a dollar figure that matches the "per-1,000-requests" metric you'll see in most competitor tables. The numbers are taken directly from the official credit-to-dollar mapping.

| Plan | Standard (1 credit) | Proxy (5 credits) | AI Extraction (8 credits) | Proxy + AI (12 credits) |
|------|--------------------|-------------------|---------------------------|--------------------------|
| **Free** | $0 / 500 req (free) | N/A (no proxy) | N/A | N/A |
| **Starter** ($19/mo) | $1.90 / 1 K | $9.50 / 1 K | $15.20 / 1 K | $22.80 / 1 K |
| **Growth** ($69/mo or $58/mo yearly) | $0.69 / 1 K | $3.45 / 1 K | $5.52 / 1 K | $8.28 / 1 K |
| **Business** ($349/mo) | $0.35 / 1 K | $1.75 / 1 K | $2.79 / 1 K | $4.19 / 1 K |

*Example*: A PHP script that pulls 10 K product pages each month, using the default HTML-LLM output, would consume 10 K credits. On the Growth plan this is priced at **$0.69 per 1 K**. Adding proxy routing (**+4 credits per request**) adds **$3.45 per 1 K**. If you also need AI-driven extraction of price and stock fields (**+5 credits per request**) adds **$5.52 per 1 K**. Compare that to Scrapingdog's flat $0.45 / 1 K for JS rendering—Geekflare's Growth plan is cheaper once you factor in the free 100 K credit allowance and the fact that you only pay for what you actually use.

Because credits **do not roll over**, you can safely size your plan to the peak month you expect. If you consistently stay under 90% of the allocated credits, Geekflare will email you a warning; once you hit 100%, requests pause until you upgrade or add a credit pack.

---

## How Geekflare Approaches PHP Web Scraping API Without Multiplier Surprises

### Single-Request Flow

1. **Endpoint** – `POST https://api.geekflare.com/webscraping`
2. **Authentication** – Send your API key in the `x-api-key` header. All 14 Geekflare APIs share this key and the same credit pool, so you can reuse the token across screenshot, SERP, and URL-to-PDF calls.
3. **Payload** – A JSON body with at least `targetUrl`. Optional fields let you select output format (`outputFormat`), enable AI extraction (`aiPrompt` or `extractionMode`), or request a proxy (`proxyCountry`, `proxyMode`).
4. **Credit Consumption** –  
   * Standard request = **1 credit** (includes headless Chrome rendering, CAPTCHA solving, and anti-bot bypass).  
   * Adding a proxy adds **+4 credits** (total 5).  
   * Enabling AI extraction adds **+7 credits** (total 8).  
   * Using both proxy and AI adds **+12 credits**.

All of those credits are deducted *before* the request is sent to the target site, so you never waste money on failed 5xx responses—those are free by design.

**Output Formats**

Geekflare supports seven formats, and you can request up to three in a single call without extra credit cost:

| Format | What You Get | Token Savings vs Raw HTML |
|--------|--------------|---------------------------|
| `html` (default) | Full rendered HTML | Baseline |
| `html-llm` | Cleaned HTML, ads & navigation stripped | ~60% |
| `markdown` | Markdown conversion of the page | ~60% |
| `markdown-llm` | Markdown with noise removed | ~75% |
| `text` | Plain text extraction | ~70% |
| `text-llm` | Text cleaned by LLM | ~85% |
| `json` | Structured JSON (useful for templates) | — |

Choosing an LLM-cleaned format can dramatically shrink the payload you need to store or feed into downstream processing, which is a hidden cost saver for PHP applications that persist results in a database.

**Rate Limits & Concurrency**

Geekflare enforces **requests per second** limits, not concurrent connections. The Free tier allows **1 req/s**, Starter **5 req/s**, Growth **10 req/s**, Business **25 req/s**, and Enterprise can be negotiated up to **1,000 req/s**. This aligns with the concurrency limits of `compare.py`, which expects request-per-second limits rather than a fixed number of parallel processes.

**Proxy Add-On Details**

The proxy feature is only available on paid plans. You can specify a `proxyCountry` (any of 194 supported countries) or let the system decide automatically (`proxyMode: "auto"`). When a direct fetch fails due to geo-blocking or bot detection, the request automatically falls back to a rotating proxy, all within the same 5-credit charge.

**AI Extraction**

Enable `aiPrompt` or `extractionMode` to let Geekflare's LLM parse the rendered page into structured data. Supported modes include:

* `prompt` – free-form LLM prompt you supply.
* `schema` – provide a JSON schema, and the API returns data that conforms to it.
* `listing`, `summary`, `sentiment`, `keywords` – ready-made extraction recipes.

The AI step adds **7 credits**, turning a 1-credit scrape into an 8-credit operation. This is ideal for PHP services that need price, stock, or review sentiment without writing custom regexes.

**Limitations Worth Mentioning**

* No interactive browser scripting (no click-through, form-fill chains).  
* The service is fully managed; you cannot self-host the scraper.  
* Proxy routing requires a paid tier; the Free plan cannot use proxies at all.  

These constraints are honest trade-offs for the simplicity and predictability Geekflare offers.

---

## Minimal Production-Ready Example

Below is a straightforward PHP snippet that fetches an Amazon product page, asks for the LLM-cleaned Markdown output, and prints the result. It uses cURL directly, which works in any PHP environment without extra extensions.

```php
<?php
$apiKey = 'YOUR_API_KEY_HERE';
$target = 'https://www.amazon.com/dp/B09G3HRMVS';

$payload = [
    'targetUrl'      => $target,
    'outputFormat'   => 'markdown-llm',   // clean markdown, reduces token count
    // optional: 'proxyCountry' => 'us', // requires paid plan
    // optional: 'extractionMode' => 'template', 'template' => 'product'
];

$ch = curl_init('https://api.geekflare.com/webscraping');
curl_setopt($ch, CURLOPT_HTTPHEADER, [
    'Content-Type: application/json',
    "x-api-key: $apiKey"
]);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($payload));

$response = curl_exec($ch);
$httpCode = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

if ($httpCode !== 200) {
    echo "Error: HTTP $httpCode\n";
    exit;
}

$data = json_decode($response, true);
if (isset($data['error'])) {
    echo "API error: {$data['error']}\n";
    exit;
}

echo "=== Clean Markdown Output ===\n";
echo $data['output']['markdown-llm'] ?? $data['output']['markdown'] ?? 'No markdown returned';
?>
```

If you want to leverage the official SDK instead of raw cURL, the Node/TS package `@geekflare/api-node` (or the Python `geekflare-api`) provides a thin wrapper, but the raw request shown above works everywhere PHP does.

---

## Choosing Which Geekflare Plan Matches Your Real Workload

| Monthly Requests (approx.) | Credits Needed (standard) | Recommended Plan | Cost (monthly) |
|----------------------------|---------------------------|------------------|----------------|
| ≤ 500 pages | ≤ 500 credits | **Free** | $0 |
| 1 K – 5 K pages | 1 K – 5 K credits | **Starter** | $19 |
| 10 K – 30 K pages | 10 K – 30 K credits | **Growth** (monthly) | $69 |
| 10 K – 30 K pages (annual) | same credits | **Growth** (yearly) | $58 |
| 100 K – 300 K pages | 100 K – 300 K credits | **Business** | $349 |
| > 300 K pages or > 25 req/s | custom | **Enterprise** | Custom |

**Proxy or AI?** Add the appropriate credit pack on top of the base plan:

* Need a rotating proxy for a subset of URLs? Add **+4 credits** per request. On Growth that's $3.45 / 1 K.
* Need AI extraction for product fields? Add **+7 credits** per request. On Growth that's $5.52 / 1 K.
* Need both? Total **12 credits** → $8.28 / 1 K on Growth.

If your traffic spikes occasionally (e.g., a seasonal sale), you can keep the base plan and purchase a **Blaze** pack ($50 for 50 K credits) that sits on top of any tier for a full year. The pack's credits are deducted before the plan's monthly allowance, letting you absorb bursts without upgrading.

---

## Managing Spiky Traffic With Credit Packs and Annual Billing

Geekflare's **annual billing** gives you two months free on the Growth tier: $58 / mo billed yearly versus $69 / mo month-to-month. The discount is applied automatically when you select the yearly option in the dashboard—no need to calculate a custom annual price.

If a request fails with a 5xx error, Geekflare does **not** charge a credit, which smooths out cost spikes caused by temporary site outages. You also get a **7-day money-back guarantee** as long as you have used less than 10% of your allocated credits. That means you can spin up a new project, test a few hundred pages, and cancel without penalty if the service isn't a fit.

When you anticipate a surge—say a flash sale that will generate 80 K additional page fetches—you have two options:

1. **Upgrade to the next tier** (e.g., from Growth to Business) for the month.
2. **Stack a credit pack**. A **Blaze** pack gives you 50 K credits for $50, which on Growth equates to $2.50 / 1 K (far cheaper than the Business per-credit rate). You can even combine multiple packs (e.g., Blaze + Inferno) to cover very large spikes without touching your base plan.

Because credits reset on the first of each billing cycle, you can treat the pack as a "buffer" that disappears after a year, leaving you with a clean slate for the next cycle.

---

## Breaking Down Every Geekflare Tier From Free to Enterprise

| Tier | Price | Monthly Credits | Rate Limit | Team Seats | Log Retention | Proxy Access | Notable Limits  | Link |
|------|-------|----------------|-----------|------------|----------------|--------------|---------------- | --- |
| **Free** | $0 | 500 | 1 req/s | 1 | 7 days | No | Credits reset monthly, no proxy  | [Start Free](https://geekflare.com/pricing/) |
| **Starter** | $19 | 10,000 | 5 req/s | 3 | 30 days | Yes (+4 credits) | Good for early-stage projects  | [Get Starter](https://geekflare.com/pricing/) |
| **Growth** | $69 / mo or $58 / mo yearly | 100,000 | 10 req/s | 5 | 30 days | Yes (+4 credits) | Most popular, priority email support  | [Get Growth](https://geekflare.com/pricing/) |
| **Business** | $349 | 1,000,000 | 25 req/s | 25 | 90 days | Yes (+4 credits) | Suitable for SaaS products  | [Get Business](https://geekflare.com/pricing/) |
| **Enterprise** | Custom | Custom | Up to 1,000 req/s | Custom | Custom | Yes (+4 credits) | Tailored SLAs, dedicated account manager  | [Contact Sales](https://geekflare.com/pricing/) |

**Common mechanics across all tiers**

* **No credit rollover** – unused credits vanish at month-end.
* **90% usage alert** – you receive an email when you cross the 90% threshold.
* **100% pause** – requests stop until you add credits or upgrade.
* **Failed requests free** – 5xx errors do not consume credits.
* **Refund policy** – cancel within 7 days and get a full refund if you've used less than 10% of your credits.

These rules keep budgeting straightforward: you know exactly how many pages you can scrape each month, and you never get surprised by hidden rollover credits.

---

## Using Geekflare Across Code, AI Agents, and No-Code Flows

**SDKs & Language Support**

* **Python** – `pip install geekflare-api` (full wrapper around the REST endpoints).  
* **Node/TypeScript** – `npm install @geekflare/api-node`.  
* **PHP** – No official SDK, but the simple cURL example above works everywhere.  
* **Go, Java, Ruby** – REST examples are provided in the docs.

All SDKs share the same API key and credit pool, so you can mix languages in a micro-service architecture without juggling multiple credentials.

### Managed Control Panel (MCP)

Geekflare ships a hosted **MCP server** at `https://mcp.geekflare.com/mcp`. It exposes 18 tools—including `webScrape`, `search`, `screenshot`, `metaScrape`, and `url2Pdf`—through a single OAuth-protected UI. You can also run the MCP locally with `npx -y @geekflare/mcp` for offline development.

**AI Framework Integrations**

* **LangChain** – `pip install langchain-geekflare` (official integration).  
* **LlamaIndex** – `pip install llama-index-tools-geekflare`.  
* **Vercel AI SDK** – package name `@geekflare/ai-sdk` (no external URL needed).  

These libraries let you embed the Web Scraping API directly into LLM agents, turning a raw page fetch into structured data that your PHP backend can consume via a simple webhook.

**No-Code Connectors**

* **n8n** – verified node at `n8n.io`.  
* **Zapier** – pre-built "Geekflare" trigger/action (`zapier.com`).  
* **Make (Integromat)** – official integration (`make.com`).  
* **Google Sheets** – you can call the API via Apps Script or the built-in "IMPORTJSON" pattern (no URL needed).

These connectors let non-developers pull product data into a spreadsheet or trigger a Slack alert when a price drops, all while consuming the same credit pool as your PHP service.

**Companion Free Tools**

Geekflare also offers a **Webpage-to-Markdown** converter and a **Scraping Feasibility Checker** (both reachable from the product page). They're handy for quick prototyping before you commit to a paid plan.

---

## A Fair Verdict on Strengths, Limitations, and Best-Fit Teams

**Strengths**

* **Predictable credit model** – No hidden multipliers; every request's cost is known up front.  
* **All-in-one rendering** – JavaScript, CAPTCHA, and anti-bot bypass are bundled into a single credit, which is rare among competitors.  
* **Generous free tier** – 500 credits let you experiment without a credit card.  
* **High uptime** – 99.9% across GCP and Cloudflare infrastructure.  
* **Strong community signals** – 4.9 overall rating (G2 4.8, Google 4.9, Product Hunt 5.0).  

**Limitations**

* **Proxy only on paid plans** – Free users cannot test geo-blocking scenarios.  
* **No interactive browsing** – No click-through or form-fill automation, which some headless browsers provide.  
* **Managed service only** – You cannot self-host the scraper, which may be a deal-breaker for highly regulated environments.  

**Best-Fit Teams**

* **Start-ups and SaaS products** that need a predictable cost model for hundreds of thousands of page fetches per month.  
* **PHP back-ends** that prefer a single-credit API and want to avoid juggling separate proxy and rendering services.  
* **AI-driven pipelines** that benefit from built-in extraction modes and can leverage LangChain or LlamaIndex integrations.  

If you require heavy interactive browsing or a self-hosted solution, a competitor like ScraperAPI or a self-managed headless Chrome cluster may be a better fit. Otherwise, Geekflare offers the most straightforward pricing and the least surprise for PHP developers.

---

## Frequently Asked Questions

**Does Geekflare roll over unused credits?**  
No. Credits reset on the first day of each billing cycle, so any leftover credits disappear at month-end.

**Can I use a proxy on the Free tier?**  
Proxy access is a paid-plan feature only; the Free tier does not include proxy routing.

**What happens if I exceed my rate limit?**  
Requests that would push you past the tier's `req/s` limit receive a 429 response and are not charged.

**Is there a way to get a refund if I change my mind?**  
Yes. Geekflare offers a 7-day money-back guarantee as long as you have consumed less than 10% of your allocated credits.

**How do I know when I'm close to hitting my credit limit?**  
Geekflare sends an email alert at 90% consumption. Once you reach 100%, the API pauses until you add credits or upgrade.

---

Explore plans on [Geekflare Pricing](https://geekflare.com/pricing/), read the [Web Scraping Docs](https://docs.geekflare.com/guides/web-scraping.md), or test the [Geekflare Web Scraping API](https://geekflare.com/webscraping/) on the card-free Free tier (500 credits/month).
