# residential rotating proxy: How Rotation Works, Sticky vs Rotating Sessions, and What You Pay Per GB or Per IP

Two questions decide whether a rotating residential proxy works out for you, and most guides only answer one of them. The first is behavioural: does your job want a fresh IP on every request, or the same IP held for ten minutes while a session stays logged in. The second is financial: are you paying per gigabyte of traffic or per IP address, because those two models can differ by several hundred dollars a month on identical workloads.

This walks through both, using 9Proxy's residential network as the concrete example, since it happens to sell both billing models side by side and lets you pick rotation mode per endpoint.

## What a rotating residential proxy actually changes

A residential IP is one that an ISP assigned to a real home connection. A rotating residential proxy is an endpoint that hands your request out through a different one of those IPs, either on every connection or after a set time window.

That matters because rate limits are almost always counted per IP. One address pulling 200 pages a minute looks like a bot. Ten thousand addresses pulling a few pages each look like a Tuesday. Rotation spreads your traffic thin enough that no single address accumulates a suspicious pattern.

Two things rotation does not do. It doesn't fix a bad browser fingerprint, so a rotating IP behind a headless Chrome with obvious automation flags still gets blocked. And it doesn't guarantee success against sites running heavy JavaScript challenges, where the outcome often depends more on the target's defences than on your pool size.

## Rotating vs sticky: the decision that actually matters

|  | Rotating | Sticky |
| --- | --- | --- |
| **IP persistence** | New IP per request or per session | Same IP for the session length you set |
| **Typical length** | Instant | Minutes to hours, depending on provider |
| **Fails when** | You need a logged-in state across requests | You push enough volume that the IP gets rate-limited |
| **Best for** | Scraping, SERP tracking, price checks, ad verification, geo-checks | Account logins, carts, checkout flows, anything that ties identity to a session |

The practical rule: if a request can stand alone, rotate it. If the second request needs the site to remember the first, hold the IP.

Most providers let you change rotation behaviour without buying a different product, and a "rotating" endpoint with a sticky session ID is often just a sticky endpoint with a shorter timer. Check that before assuming you need two plans.

## Rotation interval is the setting most people get wrong

Turning rotation up to maximum is not automatically safer. Per-request rotation on a checkout flow breaks the cart on request two. Per-request rotation on a site that fingerprints aggressively can look stranger than a slow, steady connection, because real users don't change countries between page loads.

Start with per-request rotation, log your 403 and 429 rates for a day, and only lengthen sessions when you see them climbing. If you're getting blocked with fast rotation, the answer is usually better IP quality or a slower request rate, not more IP churn.

## What to check before you pay any provider

- **Pool size and country spread.** A big number is less useful than coverage in the specific country you need.
- **Targeting granularity.** Country alone is table stakes. Country, state, city, ZIP and ISP targeting is what lets you test a local competitor's pricing properly.
- **Authentication method.** Username and password works anywhere; IP whitelisting is convenient on a fixed server but awkward on a laptop or a serverless function with a shifting outbound IP.
- **Whether rotation needs a desktop app.** Some providers route per-IP products through a local port-forwarding client. That's fine on your own machine and a problem in CI or on a cloud box.
- **Unused balance policy.** "Credits never expire" and "180-day validity" are very different deals if your projects are bursty.
- **What happens when an IP drops mid-session.** Automatic replacement keeps a job running; a hard failure keeps your data clean. Know which one you're getting.

## Per-GB vs per-IP: which billing model burns less money

Run the numbers on your own traffic before comparing headline rates, because the two models optimise for opposite workloads.

Say you fire 100,000 requests a day and each response averages 60 KB. That's roughly 6 GB a day, about 180 GB a month. On a bandwidth plan priced around $1.50/GB, that's roughly $270 a month. On a per-IP plan with unlimited bandwidth, you need enough IPs to spread the same volume, and once you have them, the data costs nothing extra.

Flip it: if you're pulling large media files or running long-session crawls that move hundreds of gigabytes through a handful of connections, per-GB pricing punishes you and per-IP pricing doesn't care.

A rough way to decide:

- **Many small requests, need lots of distinct IPs** → per-GB.
- **Few connections, heavy payloads, long sessions** → per-IP with unlimited bandwidth.
- **Both at once** → bundle plans, which exist for exactly this reason.

## How 9Proxy handles rotation

9Proxy runs a residential network of 20M+ IPs across 90+ countries and sells it two ways.

**Residential by GB** is the model that matches this keyword most directly. You buy a block of traffic, and the dashboard generates unlimited proxy endpoints against it. Each endpoint can be set to rotating mode, where the IP swaps per request or per session, or sticky mode, where the same IP holds until the session timer expires. You choose targeting by country, state, city, ZIP or ISP, authenticate with a username and password or by whitelisting your own IP, and pull endpoints in bulk as `.txt` or `.csv`, with ready-made code samples in several languages. Traffic is valid for 180 days, unlimited on Enterprise.

**Residential by IPs** works the other way. You buy a fixed number of IPs and get unlimited bandwidth on each while it's active. IPs run from a few hours up to around 24 hours, which is normal for real residential connections, and unused IPs never expire. The trade-off is setup: this line runs through the 9Proxy desktop app with local port forwarding, and there's no natural rotation. If you want rotation here, you use the Auto Rotation Proxy feature, which rotates at intervals you define on selected ports.

Both lines support HTTP/HTTPS and SOCKS5, and both sit behind the same dashboard and the same 24/7 support.

Quick judgment on which to pick. If your job is rotation-heavy, request-light on bandwidth, and you want it running from a cloud box or a browser profile with no local software, the GB line is the one you want. 👉 [Generate your first rotating residential endpoint on the GB plan](https://bit.ly/9-Proxy). If instead you're pushing big payloads through a handful of stable-looking connections, the per-IP line with unlimited bandwidth will cost you less.

## Full pricing: every current 9Proxy package

9Proxy adjusted pricing for IP-based and bundle packages on 1 June 2026. GB-based pricing was left untouched. The rates below reflect that post-adjustment structure; the checkout page is always the final word, and current promos are applied there.

| Package | Type | Price | Effective rate | Validity | Buy |
| --- | --- | --- | --- | --- | --- |
| 100 IPs | IP-based | $24 | $0.24 / IP | IPs never expire | [Get 100 IPs](https://bit.ly/9-Proxy) |
| 500 IPs | IP-based | $72 | $0.144 / IP | IPs never expire | [Get 500 IPs](https://bit.ly/9-Proxy) |
| 1,000 IPs + 500 bonus | IP-based | $126 | $0.084 / IP | IPs never expire | [Get 1,500 IPs](https://bit.ly/9-Proxy) |
| 2,500 IPs | IP-based | $210 | $0.084 / IP | IPs never expire | [Get 2,500 IPs](https://bit.ly/9-Proxy) |
| 5,000 IPs | IP-based | $360 | $0.072 / IP | IPs never expire | [Get 5,000 IPs](https://bit.ly/9-Proxy) |
| 15,000 IPs | IP-based | $720 | $0.048 / IP | IPs never expire | [Get 15,000 IPs](https://bit.ly/9-Proxy) |
| 25,000 IPs | IP-based | $863 | $0.035 / IP | IPs never expire | [Get 25,000 IPs](https://bit.ly/9-Proxy) |
| 50,000 IPs | IP-based | $1,438 | $0.029 / IP | IPs never expire | [Get 50,000 IPs](https://bit.ly/9-Proxy) |
| 100,000 IPs | IP-based | $2,300 | $0.023 / IP | IPs never expire | [Get 100,000 IPs](https://bit.ly/9-Proxy) |
| 200,000 IPs | IP-based | $4,140 | $0.021 / IP | IPs never expire | [Get 200,000 IPs](https://bit.ly/9-Proxy) |
| 500,000 IPs | IP-based | $8,625 | $0.018 / IP | IPs never expire | [Get 500,000 IPs](https://bit.ly/9-Proxy) |
| 5 GB | GB-based | $15 | $3.00 / GB | 180 days | [Get 5 GB](https://bit.ly/9-Proxy) |
| 50 GB + 5 GB bonus | GB-based | $105 | $2.10 / GB | 180 days | [Get 55 GB](https://bit.ly/9-Proxy) |
| 100 GB | GB-based | $150 | $1.50 / GB | 180 days | [Get 100 GB](https://bit.ly/9-Proxy) |
| 200 GB | GB-based | $200 | $1.00 / GB | 180 days | [Get 200 GB](https://bit.ly/9-Proxy) |
| 1,000 GB | GB-based | $800 | $0.80 / GB | 180 days | [Get 1,000 GB](https://bit.ly/9-Proxy) |
| 2,000 GB | GB-based | $1,500 | $0.75 / GB | 180 days | [Get 2,000 GB](https://bit.ly/9-Proxy) |
| 3,000 GB | Enterprise GB | $2,160 | $0.72 / GB | No expiry | [Get 3,000 GB](https://bit.ly/9-Proxy) |
| 6,000 GB | Enterprise GB | $4,200 | $0.70 / GB | No expiry | [Get 6,000 GB](https://bit.ly/9-Proxy) |
| 10,000 GB | Enterprise GB | $6,800 | $0.68 / GB | No expiry | [Get 10,000 GB](https://bit.ly/9-Proxy) |
| 100 IPs + 5 GB | Bundle | $30 | — | Traffic valid 180 days | [Get the Starter bundle](https://bit.ly/9-Proxy) |
| 1,500 IPs + 50 GB | Bundle | $180 | — | Traffic valid 180 days | [Get the mid bundle](https://bit.ly/9-Proxy) |
| 5,000 IPs + 500 GB | Bundle | $720 | — | Traffic valid 180 days | [Get the Pro bundle](https://bit.ly/9-Proxy) |

A few things worth noticing in that table. The per-IP price falls from $0.24 at the entry tier to $0.018 at 500,000 IPs, so scaling is where the value is. The GB line bottoms out at $0.68/GB on the 10,000 GB Enterprise package, and that's the package that also removes the expiry window. The bundle tiers beat buying the IPs and the traffic separately, which is the point of them.

## Getting a rotating endpoint running

The GB line doesn't require anything installed locally, which is the main operational difference.

1. Create an account and pick a GB package. 👉 [Start with the 5 GB package if you're just testing](https://bit.ly/9-Proxy).
2. In the dashboard, choose your authentication method: username/password, or whitelist your server's outbound IP.
3. Open the Proxy Generator and set your target location — country, state, city, ZIP or ISP.
4. Pick rotating mode for per-request rotation, or sticky mode and set the session length.
5. Export the endpoints as `.txt` or `.csv`, or copy the generated code sample for your language, and paste the credentials into your scraper, browser profile or automation tool.

If you're on the IP-based line instead, the flow is different: install the 9Proxy app, forward a local port to an IP, and configure Auto Rotation Proxy on the ports where you want timed rotation.

## Where 9Proxy is weaker

It's worth knowing the edges before you commit.

The pool is 20M+ IPs. Bright Data and Oxylabs advertise networks several times that size, so if you need very deep coverage in a small or unusual region, a bigger provider may have more headroom. Review coverage also flags the IP-based line's desktop app as more awkward for multi-device setups than a browser extension or a pure API, and reports that residential IPs on 9Proxy have been blocked by major streaming platforms, so it's not the pick for streaming unblocking specifically. Social media account management results are reported as inconsistent across platforms, which is normal for residential networks but worth planning around.

One more: GB balances carry a 180-day validity window on the standard packages. If your usage is genuinely sporadic, that window is the constraint to weigh, and it's one of the reasons the Enterprise GB tier exists.

## Test before you commit

9Proxy runs a limited trial for new users, and availability varies, so the practical route is to ask support whether it's open and specify whether you want an IP-based or GB-based trial. Sign-up itself doesn't require a card.

If you'd rather test with real money at small scale, the 5 GB package at $15 and the 100 IP package at $24 both let you measure success rate and latency against your actual targets before you scale. Reviews also note that selected payment methods carry an extra 5% discount or a 5% product bonus, which is worth checking at checkout.

## FAQ

**Is a rotating residential proxy the same as a rotating datacenter proxy?**
The rotation mechanism is the same. The difference is the IP's origin. Residential IPs come from real ISP-assigned home connections, which is why they survive detection that datacenter ranges don't. The cost is that residential IPs are slower and less stable, and they can drop mid-session.

**How often should I rotate?**
As infrequently as your target site allows. Per-request rotation for stateless scraping, timed sessions wherever the site tracks a session. Watch your 403 and 429 rates and adjust from there.

**Can I run these in an anti-detect browser?**
Yes. Users on review sites report using 9Proxy with tools like Dolphin Anty and AdsPower. The GB line's username/password auth is the straightforward fit since it needs no local app.

**Do unused GB expire?**
On standard GB packages, 180 days from purchase. Enterprise GB packages have no expiry.

**Do the IP-based plans rotate automatically?**
Not by default. They're built for holding an IP through a session. Rotation on that line comes from the Auto Rotation Proxy feature, which rotates at intervals you configure on selected ports.

**What payment methods work?**
Cards, bank cards, crypto including USDT, BTC, ETH, LTC and DOGE, plus Alipay, Apple Pay and Google Pay.

The short version: if your workload is a lot of small requests that need a lot of different IPs, buy GB and set rotation mode to rotating. 👉 [Open the 9Proxy dashboard and check which package fits your traffic](https://bit.ly/9-Proxy). If it's a smaller number of connections moving serious bandwidth, buy IPs and forget the traffic meter exists.
