# Smartproxy Alternative: Do You Still Need One, What 100 GB Costs at Decodo Now, and When a Scraping API Beats Raw Proxy Bandwidth (All ScraperAPI Plans Compared)

Smartproxy became Decodo on 22 April 2025. The old domain has been issuing a permanent 301 redirect to decodo.com, last checked on 1 November 2026, and Decodo states that accounts, subscriptions, infrastructure and existing proxy endpoints carried over through the rename. A working Smartproxy integration keeps running as it is, so rebuilding it because the logo changed buys migration risk and fixes nothing.

The billing model underneath is worth a second look. "Smartproxy alternative" gets used for two purchases that barely resemble each other: residential bandwidth billed by the gigabyte, and finished, parseable pages billed by the request. Decodo sells the first. ScraperAPI sells the second, with residential and mobile IPs folded into a credit system rather than metered as traffic. Choosing between them without knowing which one you're actually buying is how people end up paying $275 a month for 100 GB they only half use.

Below: what changed in the rename, every ScraperAPI plan and the credit maths that decides your real cost, Decodo's current published prices, and the scenarios where each one wins.

> **Quick pick**
> - **You need gigabytes of rotating residential IPs.** Decodo stays, and the 100 GB tier is **$275/month** ($2.75/GB). Cheaper per-GB options exist: Webshare at $225/month for 100 GB, Shifter at $2.50/GB.
> - **You need pages, JSON or SERP results and don't want to manage proxies.** ScraperAPI's **Hobby plan is $49/month for 100,000 credits**; **Scaling at $475/month** adds 5M credits and pay-as-you-go; **Business at $299/month** is the cheapest tier with country-level geotargeting outside the US and EU.
> 👉 [See every ScraperAPI plan and its current credit allowance](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## What changed on 22 April 2025, and what didn't

The rename is administrative. Smartproxy launched in 2018 as a product of UAB "Data troops", a Vilnius company, selling residential traffic per GB either by subscription or pay-as-you-go. On 22 April 2025 the service took the Decodo name and kept the company, team and network. Existing connection endpoints continue to work, which means the hostname, port, username flags and allowlist entries in your scraper don't need touching.

Two housekeeping details are worth knowing. Decodo states that smartproxy.org and smartproxy.cn are not its domains, so credentials entered there go somewhere else. And the redirect from smartproxy.com is a 301, not a temporary hop, so old bookmarks and documentation links land on the current site rather than breaking.

If your Smartproxy setup is failing, the cause is almost certainly something other than the brand: an expired subscription, a rotated password, a target site that changed its defences, or a plan whose location controls never covered the city you now need. None of those are solved by a new vendor.

## ScraperAPI's full plan grid

ScraperAPI prices in API credits, and the pricing page was verified in October 2026. Seven tiers, all monthly by default with an annual option.

| Plan | Credits per month | Concurrent threads | Monthly | Annual (per month, billed yearly) | Start |
| --- | --- | --- | --- | --- | --- |
| Hobby | 100,000 | 20 | $49 | $44.10 | [ Hobby plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | 1,000,000 | 50 | $149 | $134.10 | [ Startup plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | 3,000,000 | 100 | $299 | $269.10 | [ Business plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling (most popular) | 5,000,000 | 200 | $475 | $427.50 | [ Scaling plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | 10,500,000 | 300 | $975 | $877.50 | [ Professional plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | 21,500,000 | 500 | $1,975 | $1,777.50 | [ Advanced plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | 22,000,000+ | 500+ | Custom | Custom | [ Request an Enterprise quote](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Annual billing is exactly 10% below monthly on every published tier. The features that separate the plans sit outside the headline numbers:

| Plan | Geotargeting | Analytics history | Pay-as-you-go overage | Support |
| --- | --- | --- | --- | --- |
| Hobby | US and EU only | Up to 30 days | No | Standard |
| Startup | US and EU only | Up to 30 days | No | Standard |
| Business | Global, country level | Unlimited | No | Standard |
| Scaling | Global, country level | Unlimited | Yes | Standard |
| Professional | Global, country level | Yes | Yes | Priority |
| Advanced | Global, country level | Unlimited | Yes | Priority routing |
| Enterprise | Global, country level | Unlimited | Yes | Dedicated team, Slack |

Every tier includes JavaScript rendering, the premium residential and mobile pools, anti-bot bypass, JSON autoparse, rotating pools, custom headers, CAPTCHA handling, automatic retries, unlimited bandwidth and a 99.9% uptime commitment. The differences are volume, concurrency, geography and what happens when you run out.

## A credit is not a page: the multiplier that decides your real cost

The number on the plan is a credit allowance, and the cost of a request depends on the domain you point it at. A standard page costs 1 credit. Amazon costs 5. Google and Bing, including subdomains, cost 25. LinkedIn costs 30. Sites sitting behind Cloudflare, DataDome or PerimeterX add 10 credits per request when ScraperAPI activates bypass logic.

Run the Hobby plan at $49 against that table and the effective cost per 1,000 successful requests moves a long way:

- Standard pages: 100,000 requests, **$0.49 per 1,000**
- Amazon: 20,000 requests, **$2.45 per 1,000**
- Cloudflare-protected pages with bypass: roughly 9,090 requests, **$5.39 per 1,000**
- Google or Bing SERPs: 4,000 requests, **$12.25 per 1,000**
- LinkedIn: about 3,333 requests, **$14.70 per 1,000**

The premium pools multiply again. Setting `premium=true` charges 10 credits per successful request, 25 when you combine it with rendering. The `ultra_premium=true` pool charges 30 credits, or 75 with rendering enabled. On the $49 Hobby plan that last combination buys you about 1,333 requests for the month. At the other end, Advanced at $1,975 for 21.5M credits works out to roughly $0.09 per 1,000 standard pages.

Two consequences follow. A SERP-tracking project on Hobby has an effective ceiling of 4,000 queries a month, which is a small monitoring job rather than a data pipeline. And the Domain Cost Estimator inside the dashboard is the only authoritative per-URL figure, because the multipliers stack differently by site.

👉 [Check which plan's credit allowance covers the domains you actually scrape](https://www.scraperapi.com/pricing/?fp_ref=coupons)

The overage rules matter as much as the multipliers. Pay-as-you-go exists from Scaling upward. On Hobby, Startup and Business, exhausting your credits returns a 429 rather than a bill: requests are refused, not queued and not charged. If you scrape in bursts near a tier boundary, that is either a safety net or a hard stop, depending on your deadlines. Credits also reset at each renewal date, and anything unused is gone.

## What Decodo charges in November 2026

The residential subscription ladder runs from **$3.75/GB at 3 GB down to $2.75/GB at 100 GB**, with an enterprise band of **$2.50/GB at 250 GB to $2/GB at 1 TB**. At those rates, 100 GB costs **$275 per month** and 1 TB costs **$2,000 per month**, before VAT.

Pay-as-you-go residential is **$4/GB**, bought GB by GB from a wallet, and here is the restriction that catches people: an account cannot hold a monthly subscription and pay-as-you-go at the same time. It is one or the other, so a hybrid of "subscription for the baseline, wallet for spikes" isn't available.

Mobile proxies are currently discounted 50%, putting the 2 GB tier at **$3.75/GB** and 100 GB at **$2.75/GB**, which is $275 for that 100 GB. The pre-discount rates are **$7.50 to $5.50/GB**. When the promotion ends, the same 100 GB at the listed $5.50/GB becomes $550 a month, so budget the renewal at the list rate rather than the promotional one. For context, IPRoyal lists 100 GB of mobile at $520, which makes the discounted Decodo rate the cheaper of the two and the full rate roughly comparable.

Dedicated static ISP proxies are priced per IP per month plus VAT: **$3.33 at 3 IPs**, $2.90 at 10, $2.80 at 20, $2.70 at 50, $2.60 at 100 and **$2.50 at 200**. These are fixed addresses on a monthly term, a different product from the rotating residential gateway, and the pricing reflects that.

Residential trials run **3 days with 100 MB of traffic**, enough to check authentication and confirm that a target site accepts the pool, not enough to measure a success rate on a hard target.

Decodo publishes 115M+ residential IPs across 195+ locations. That is the vendor's own figure and it describes the network, not what your account will see in one city at one moment.

## Per-GB alternatives, if gigabytes are really what you need

If the requirement is raw residential bandwidth, compare on a like-for-like monthly basis rather than on the lowest advertised rate.

| Provider | 100 GB residential | 1 TB residential | Notes |
| --- | --- | --- | --- |
| Decodo | $275/mo | $2,000/mo (enterprise tier) | Pay-as-you-go $4/GB, cannot combine with a subscription |
| Webshare | $225/mo | $1,500/mo | $1.40/GB at the 3,000 GB tier |
| Shifter | $250/mo ($2.50/GB) | $1,000/mo ($1/GB) | States no annual lock-in |
| Oxylabs | $500/mo Flexible (up to 125 GB at $4/GB) | From $2,500/mo at 1K+ GB | Enterprise band starts at 1K+ GB |
| IPRoyal | No 100 GB tier; largest standard plan is 10 GB at $5.25/GB | Custom at $1.75/GB from 10 TB | Traffic does not expire |
| Bright Data | No 100 GB tier; smallest is 141 GB at $499/mo with a promo code (list $987) | Largest listed is 798 GB at $1,999/mo with code (list $3,990) | Promotional pricing runs 3 months |

Figures are published list prices read on 1 November 2026, excluding VAT, with promotional conditions noted where the vendor flags them. Vendors that price by IP or by seat rather than by GB sit outside this table and are not comparable on a per-gigabyte basis.

The pattern is that Decodo's 100 GB rate is mid-market, and the gap widens at volume: 1 TB costs $2,000 a month at Decodo against $1,000 at Shifter and $1,500 at Webshare. If you move a terabyte a month, that difference is a real annual line item rather than a rounding error.

## Match the plan to the mismatch, not to the headline

Write down what is broken before opening a pricing page. "Cheaper" is not a requirement until volume, expiry, tax and failed requests are defined, and several common complaints point to specific products rather than to a different vendor.

**Irregular traffic that wastes a monthly allowance.** A recurring GB subscription suits steady volume. For spiky jobs, Decodo's $4/GB pay-as-you-go avoids paying for capacity you don't consume, with the caveat that it excludes holding a subscription. On the API side, pay-as-you-go only unlocks at Scaling ($475/month), so small unpredictable jobs are cheaper to run in GB terms until the request volume justifies the jump.

**You need a fixed list of addresses, not a rotating gateway.** Rotating residential is the wrong product for account work, allowlisting or anything that requires the same exit twice. Decodo's dedicated ISP tier at $2.50 to $3.33 per IP per month is the relevant line; ScraperAPI doesn't sell static IP lists at all, so this requirement rules it out.

**You need city, ZIP, coordinate or ASN-level control.** Confirm the grain exists in the product you're buying before you pay. ScraperAPI offers US and EU targeting on Hobby and Startup, and country-level global targeting from Business upward. Decodo publishes country and finer controls with product-specific rules. Neither vendor's marketing page will tell you whether your specific combination works; the dashboard will.

**You want structured output rather than an IP.** This is where the two categories genuinely diverge. Managing a residential pool means owning the retry logic, the parsing, the CAPTCHA handling and the block detection. A scraping API prices all of that into the credit. For a two-person team scraping 50,000 product pages a month, the engineering time usually costs more than the difference between $0.49 and $2.45 per 1,000 requests.

👉 [Compare Business and Scaling, where country-level targeting and pay-as-you-go start](https://www.scraperapi.com/pricing/?fp_ref=coupons)

**Only the rename is bothering you.** Stay on Decodo. There is no migration to do.

## Run one controlled pilot before you move traffic

A switch should be decided by a test, not by a pricing table. The test has to hold everything except the provider constant: same target URLs, same client version, same protocol, DNS, TLS settings, headers, cookies, request mix, country, concurrency, timeouts, retry ceiling, time window and validation rule.

Score the pilot against gates you set before it starts:

- **Location accuracy.** Requested versus observed country, region, city, ZIP and ASN.
- **Valid-result rate.** Count raw valid, incomplete, challenged, denied and transport-failed responses separately.
- **Latency.** p50 and p95 for the full task, with failures reported outside the latency figures.
- **Session continuity.** Whether the exit held for the length of the workflow.
- **Traffic reconciliation.** Client-side bytes against the provider meter, and the gap between them.
- **Cost per usable result.** Plan price, tax, retries, compute, review time and validated outputs, divided by the outputs.
- **Operations.** Setup time, secret handling, and whether the support path actually resolved a redacted ticket.

Then cut over in the order that keeps failures attributable: freeze the current baseline with a redacted successful request, build a separate candidate configuration instead of overwriting the incumbent's credentials, map country, city, ASN, rotation and authentication semantics explicitly, and send a small canary slice while the old provider is still live. Log DNS, TCP, TLS, proxy authentication, destination status, content validation and billing evidence as separate failure hops. Promote on the declared gate.

One rule belongs outside the test plan: a 401, 403, 429, CAPTCHA, account warning or explicit denial is a stop, backoff or authorisation-review signal. Cycling providers or sessions in response turns a temporary block into a policy problem.

## Pool size is not inventory

Vendors publish pool totals, coverage counts, uptime statements and success rates, and those figures are rarely measured the same way twice. A monthly address count is not the number of unique exits available to one account in one city at one moment. A provider-wide success claim says nothing about your destination, your validation rule, your timeout, your retry policy, your concurrency or your test window.

This matters most for buyers who pick a network because the landing page says 100M+ IPs. The number that decides your outcome is the valid-result rate on your target list at your concurrency, which only your own pilot produces. The same applies to the 115M+ figure Decodo publishes: it describes a network, and networks are shared.

## Three rules for the decision in front of you

**Gigabytes are the product you need.** Stay on Decodo unless your monthly volume is large enough for the per-GB gap to matter. At 100 GB the difference between $225 and $275 is worth a conversation; at 1 TB the difference between $1,000 and $2,000 is worth a migration. Confirm which tier the quote assumes, because Bright Data's headline requires a promo code and Decodo's mobile rate reverts to $5.50–$7.50/GB.

**Requests are the product you need.** Price the plan against your actual domain mix, not against the credit number. Hobby at $49 covers 100,000 standard pages or 4,000 Google queries, and those are the same $49. If your mix is heavy on search engines, marketplaces or bot-protected sites, the tier you need is one or two steps above the one the page count suggests.

**Both are true.** Large teams often keep a GB-metered pool for general browsing and an API for the hard targets, because the API's premium pool pricing (10 to 30 credits per request) gets expensive as a general-purpose tool but stays predictable on the 5% of pages that actually need it.

👉 [Start on the ScraperAPI plan that matches your monthly request count](https://www.scraperapi.com/pricing/?fp_ref=coupons)

Two things to check before any card gets charged: whether the rate you're looking at is the renewal rate or a promotional one, and whether unused allowance survives the billing period. On both sides of this comparison, the answer to the second question is no.
