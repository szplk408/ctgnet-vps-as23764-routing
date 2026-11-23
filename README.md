# CTGNet VPS Guide: What China Telecom's AS23764 Route Really Is, How It Compares to CN2 GIA, and What to Check Before You Pay

A CTGNet VPS is a server whose China-bound traffic rides China Telecom's AS23764 backbone, the newest of the carrier's premium routes into the mainland. In price and in day-to-day behavior it sits at roughly the same level as CN2 GIA on AS4809, which is why providers selling both tend to describe them as the same grade. The name refers to a routing tier, not to a class of hardware: you are still renting a KVM slice in some datacenter, and the part worth paying extra for is the path your packets take once they leave that datacenter.

So is it worth buying? If a meaningful share of your users connect from Chinese networks and your current box turns to sludge between 20:00 and 23:00 Beijing time, yes. The premium tier is the only one that reliably holds latency and packet loss through the evening peak. If your audience is mostly outside China, you gain nothing and should spend the money on CPU instead. If your service attracts attacks, the premium tier is the wrong tool for reasons covered further down.

## What Is a CTGNet VPS, and Where Does It Sit in China Telecom's Lineup?

China Telecom sells international transit into mainland China in four broad tiers, and the price difference between the top and bottom of that ladder is enormous.

**AS4134 (ChinaNet, often called 163)** is the cheap default. Vast capacity, present on nearly every cloud provider's default route into China, and heavily congested at peak. It also absorbs attacks well, which matters later.

**AS4809 CN2 GT (Global Transit)** was built to fix that congestion. Since around 2019 it has degraded toward AS4134-level behavior while still costing noticeably more, which makes it the worst value of the four.

**AS4809 CN2 GIA (Global Internet Access)** is the premium product. Transit pricing has been quoted as high as $120 per megabit in some markets, so a full 1 Gbps commit could theoretically run into six figures a month. Capacity is deliberately limited, and that limit is exactly why it stays stable when cheaper routes fall apart.

**AS23764 CTGNet** is the newest addition, run by China Telecom Global, the carrier's international arm. In BandwagonHost's own operational write-up, CTGNet is "equivalent to CN2 GIA in both pricing and performance" — same low latency, same stability, on a fresh AS number so the legacy CN2 GIA pipes don't get squeezed dry. Its overseas footprint is smaller than CN2's; Chinese-language write-ups from 2024 counted a handful of nodes across Asia, Europe and North America, with Hong Kong and Tokyo among the first to show up in retail VPS products.

One clarification worth keeping in mind while you shop. Some providers describe CTGNet as "formerly China Telecom CN2 GIA," which reads as though the carrier renamed an existing network. AS23764 and AS4809 are separate networks run by separate parts of the carrier, and treating the two as identical will lead you to expect a discount that isn't there. Expect CTGNet to be priced like GIA, because it is.

That price level is also your first sanity check. As one write-up on China-optimized hosting puts it: "A lot of budget options are just repackaged ChinaNet with a fancy label." If a listing advertises CTGNet at a price that looks like a 163 plan, ask for the looking glass before you pay.

[👉 check the current CTGNet plan list and prices](https://666clouds.com/aff.php?aff=1828)

## What a Plan Has to Get Right to Deserve the Name

A China-optimized plan is only as good as its weakest carrier path, and that gives you four things to check in order.

**All three carriers, not just Telecom.** The combination to look for is CTGNet (AS23764) for China Telecom, China Unicom Premium (AS9929 or AS10099) for Unicom, and CMIN2 (AS58807) for China Mobile. V.PS, sa.net and BandwagonHost all describe their China-optimized products this way, and the AS numbers line up across all three. A plan that only optimizes Telecom covers roughly one slice of the Chinese audience and leaves the rest on whatever the datacenter's default transit happens to be.

**Which plans in the lineup actually get it.** V.PS rolled out CTGNet, CUP and CMIN2 in August 2026 on its Performance KVM VPS line in Tokyo and Singapore only. Other tiers in the same cities, and every other location, were left out. Existing customers did not have to change anything, since the routing happens at the network layer rather than in the server config, and there was no surcharge. The pattern is common: the optimized route is attached to a specific product line, and the cheaper tier in the same building may not have it.

**Where the server sits.** A Los Angeles box on a three-carrier premium route lands around 120 ms from mainland China and stays steady through the evening. An Asia-located box on the same grade of route can drop under 50 ms, at roughly double the price per gigabyte of traffic. Tokyo and Singapore also differ from each other: Tokyo has dense peering and direct upstreams to all three Chinese carriers, while Singapore's pitch is proximity plus a jurisdiction outside mainland China's regulatory reach, which matters for some businesses and not at all for others.

**Whether it costs extra.** V.PS charges nothing additional for the optimized routing on the covered plans. Ask the same question of any provider, because "China optimized" has been used to justify a surcharge on hardware that is otherwise identical.

Then verify it yourself rather than trusting the badge. Run a traceroute or an mtr from a Chinese IP, or use a public looking glass, and compare against what standard international transit to the same destination looks like. A genuinely optimized path shows fewer hops and steadier latency, most visibly during the hours that would normally be worst. For a sense of what "good" looks like, sa.net publishes a Tokyo-to-Shanghai sample on its product page: 30 ms on CTGNet, 35 ms on CUP, 35 ms on CMIN2, all at 0.0% loss. Those are the vendor's own estimates and the page labels them "for reference only," so treat them as an illustration of the shape of a healthy result, not as a promise for your own line.

## The Plans You Can Actually Compare Right Now

Three product shapes show up repeatedly in this segment, and each one has published specs you can check. The tables below reproduce what the vendors display.

**Los Angeles, three-carrier premium route (BandwagonHost E-Commerce, DC9).** Eight 10 GbE CN2 GIA/CTGNet links across two datacenters, with China-bound traffic handed to CN2 GIA, CMIN2 and China Unicom Premium. Plans can be migrated between DC6, DC9, JPOS_1, EUNL_9 and others without losing data.

| Plan | RAM | CPU | Storage | Traffic | Port | Price |
| --- | --- | --- | --- | --- | --- | --- |
| EC-1GB | 1 GB | 2 cores | 20 GB SSD | 1 TB/mo | 2.5 Gbps | $49.99/quarter · $169.99/yr |
| EC-2GB | 2 GB | 3 cores | 40 GB SSD | 2 TB/mo | 2.5 Gbps | $89.99/quarter · $299.99/yr |
| EC-4GB | 4 GB | 4 cores | 80 GB SSD | 3 TB/mo | 2.5 Gbps | $56.99/mo · $549.99/yr |
| EC-8GB | 8 GB | 6 cores | 160 GB SSD | 5 TB/mo | 5 Gbps | $86.99/mo · $879.99/yr |
| EC-16GB | 16 GB | 8 cores | 320 GB SSD | 8 TB/mo | 5 Gbps | $159.99/mo · $1,599.99/yr |
| EC-32GB | 32 GB | 10 cores | 640 GB SSD | 10 TB/mo | 10 Gbps | see official page (listings differ) |
| EC-64GB | 64 GB | 12 cores | 1 TB / 1280 GB (listings differ) | 12 TB/mo | 10 Gbps | $549.99/mo · $5,399.99/yr |

**Asia, same grade of route, entry tier (BandwagonHost Ultra).** All four locations ship 2 GB RAM, 2 cores, 40 GB SSD and 500 GB monthly traffic; larger sizes are listed on the official order page.

| Location | Datacenter | Port | Price |
| --- | --- | --- | --- |
| Hong Kong | HKHK_8 | 1 Gbps | $89.99/mo · $899.99/yr |
| Tokyo | JPTYO_8 | 1.2 Gbps | $89.99/mo · $899.99/yr |
| Osaka | JPOS_6 | 1.5 Gbps | $49.99/mo · $499.99/yr |
| Singapore | SG_8 | 1.5 Gbps | $49.99/mo · $499.99/yr |

**Tokyo or Singapore, three-carrier KVM (sa.net).** AMD Ryzen 9950X, NVMe, 1 Gbps port, daily backups, one IPv4 plus a /64 IPv6, Linux templates only.

| Plan | RAM | CPU | Storage | Traffic | Price |
| --- | --- | --- | --- | --- | --- |
| Premium | 4 GB | 2 vCPU | 40 GB NVMe | 1 TB/mo | $49/mo |
| Ultra | 8 GB | 2 vCPU | 80 GB NVMe | 2 TB/mo | $99/mo |
| Max | 16 GB | 4 vCPU | 160 GB NVMe | 4 TB/mo | $199/mo |

V.PS sells the same three-carrier combination on its Performance KVM VPS line in Tokyo and Singapore with no routing surcharge, though its plan sizes and prices change often enough that the official cart is the only place worth reading.

Prices above are what the vendors displayed when I went through their pages; confirm on the live order page before you pay.

## Which Plan Fits Which Reader

Personal projects, a self-use proxy, a small site: take the entry tier on the Los Angeles line. The routing does not get worse because the plan is small, so a 1 GB or 2 GB box gets the same three-carrier path as the 64 GB one.

A production service with real Chinese users: move up to the 4 GB or 8 GB tier. The jump from a 2.5 Gbps to a 5 Gbps port is about concurrency during traffic spikes, not about single-connection speed, and it is the difference between a page that loads and a queue that forms.

Latency-sensitive work such as game servers, live trading or real-time collaboration: go to Asia and accept the bill. Hong Kong gives the shortest physical path into the mainland, Osaka and Singapore are the cheaper Asian entries at $49.99/mo, and Tokyo sits in between on price with the best-developed peering of the three.

Budget-first, latency-second: stay in Los Angeles. Around 120 ms with a stable evening is fine for content, downloads and most back-office tools, and the per-gigabyte cost is roughly half of what the Asian locations charge.

[👉 compare the CTGNet plan options side by side](https://666clouds.com/aff.php?aff=1828)

## Where CTGNet Falls Short, and Who Should Skip It

**It does not survive DDoS attacks.** BandwagonHost states this plainly in its own documentation: the limited capacity that keeps CN2 GIA and CTGNet stable also means there is no headroom to absorb a large attack. When one arrives, the response is IP nullrouting, not scrubbing. If you run something that attracts griefing, a cheap AS4134 ChinaNet plan with DDoS protection layered on top will stay online through an attack that would take a premium route offline.

**Optimized routing does not shorten distance.** V.PS put the limit better than any marketing page could: "optimized routing improves the network path, it doesn't change the underlying physics of distance entirely, and it won't deliver identical performance to every region within mainland China equally." The same announcement adds that it "doesn't substitute for adequate server resources on your end." A well-routed 1 GB box running a heavy application is still slow.

**Three carriers, three different experiences.** Each Chinese carrier runs a largely separate network, so CTGNet, CUP and CMIN2 behave differently from one another even on the same server. Traffic from Telecom users and traffic from Mobile users will not feel identical, and no provider can flatten that out.

**Users outside mainland China see no improvement.** V.PS says so directly about its own rollout, and the same holds for any China-optimized plan: if your audience is in Europe or the Americas, you are paying for a route they never touch.

**Some plans cannot be downgraded.** sa.net handles upgrades by support ticket only, from Premium to Ultra or Max, with no self-service and no downgrade afterward. Its refund window is 14 days and requires under 10 GB of bandwidth used, an IP that has not been blacklisted, and no AUP violation. BandwagonHost offers a 30-day refund, which is more forgiving but still a deadline.

**Annual pricing hides a prepayment.** Monthly averages quoted on comparison sites are usually an annual figure divided by twelve, paid up front. If the route degrades in month three, you have already spent the money. Paying quarterly or monthly costs more per unit but caps the damage.

## Before You Order: Four Questions Worth Answering First

### Do I have to reconfigure anything on the server?

No. The routing change happens at the network layer, on the provider's side. Existing V.PS customers on covered plans had CTGNet, CUP and CMIN2 active before the announcement went out. New orders get the same routing from day one.

### Will Telecom, Unicom and Mobile users all get the same result?

No, and any provider claiming otherwise is overselling. The three carriers run separate networks with separate behavior. What the plan buys you is a premium path on all three, so nobody falls back to congested default transit.

### Can I test the route before committing?

Ask for a test IP or use the provider's public looking glass, then run mtr from a Chinese vantage point during the evening peak rather than at 10 a.m. Compare hop count and latency jitter against a standard international route to the same city. That test tells you more than any spec sheet.

### Is there a coupon code?

I did not find an official coupon published for the provider linked at the end of this article, so I'm not going to list one; codes circulating on coupon sites are unverified and often expired. The BandwagonHost code BWHCGLUKKB does appear on third-party promotional pages as a recurring discount, quoted at 6.77% on one and 6.78% on another, which is a good reminder to check the figure in the cart yourself. Applied to the $49.99 quarterly entry plan, either rate lands at roughly $46.61 per quarter on every renewal.

The cheapest way to buy a CTGNet plan is to start on the shortest billing cycle the provider offers, test the route from your own users' networks for a month, and only then move to a longer term. Routing quality is the entire product here, and one month of your own mtr output is worth more than any table.

[👉 see the live CTGNet pricing before you commit](https://666clouds.com/aff.php?aff=1828)
