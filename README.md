# hosted private cloud services: what they actually cost, which plan fits your workload, and the limits nobody mentions

Hosted private cloud services give you a dedicated or isolated cloud environment operated by a provider, so you get cloud-style self-service and APIs without sharing hardware with strangers. The catch is that pricing varies wildly between providers, and the sticker price often hides the real bill: egress fees, metered storage, and management costs.

That's why this guide focuses on numbers. We'll use Sharktech, a Las Vegas-based infrastructure company founded in 2003 with data centers in Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam, as the worked example — because its cloud pricing is unusually transparent, and it publishes hourly rates for every resource. Whether you end up choosing Sharktech or a bigger name, the cost math below applies to almost any provider you're evaluating.

> **Quick picks**
> - Testing OpenStack or running a small production stack: **Public Cloud Small**, 4 cores / 8GB RAM, **$39/month** base.
> - Steady workload with a fixed budget: **Dedicated Cloud** — same platform, fixed allocation, fixed price, no hourly overages.
> - Serious workload leaving AWS or Azure: **Enterprise**, 64 cores / 128GB RAM / 5TB SSD, **$499/month**.
> 👉 [Configure the Small plan and check current pricing on Sharktech](https://bit.ly/SharKtech)

## What "hosted private cloud" actually means — and Sharktech's three flavors

The term covers anything from a single-tenant VM cluster to fully dedicated hardware. Providers blur the definition on purpose, so it helps to split it into three distinct products, which is exactly how Sharktech structures its lineup:

1. **Public Cloud.** A shared pool of compute, storage, and networking built on OpenStack, billed as a monthly base plus metered usage above your included allocation. It's the cheapest entry point and the most flexible — you can burst resources when you need them.
2. **Dedicated Cloud.** Same OpenStack platform, but you pay a fixed monthly price for a fixed resource allocation. Sharktech's own phrasing: "If you pay for 8 cores, you get 8 cores. No more, no less." No hourly billing, no automatic overages. This is the plan type most people mean when they search for hosted private cloud with a predictable budget.
3. **Private Cloud.** Fully dedicated physical infrastructure, designed and built by Sharktech's engineers around your requirements. Hardware resources are exclusively allocated to your cloud, and you can either host it with Sharktech as an operating expense or own the hardware yourself as a capital purchase, colocated anywhere — including your own office or data center.

One useful detail: Sharktech's Public and Dedicated Cloud run on the same hyperconverged OpenStack infrastructure. The only difference is billing. So if you start on Public Cloud and your usage stabilizes, moving to Dedicated Cloud doesn't mean migrating platforms.

## Full plan and pricing table

All prices in USD, monthly billing, from Sharktech's official cloud pages and cost calculator. Public Cloud plans include a base resource allocation; usage above it is metered hourly at published rates.

| Plan | Base config | Monthly base price | Usage above allocation | Order |
| --- | --- | --- | --- | --- |
| Small | 4 vCPU / 8GB RAM / 300GB SSD / 20TB out | $39.00 | Hourly metered (rates below) | [Configure Small](https://bit.ly/SharKtech) |
| Medium | 8 vCPU / 16GB RAM | $79.00 | Hourly metered | [Configure Medium](https://bit.ly/SharKtech) |
| Large | 32 vCPU / 64GB RAM | $249.00 | Hourly metered | [Configure Large](https://bit.ly/SharKtech) |
| Enterprise | 64 vCPU / 128GB RAM / 5,000GB SSD | $499.00 | Lower unit rates, no preset ceiling | [Configure Enterprise](https://bit.ly/SharKtech) |
| Custom | Configurable | Contact sales | Negotiated | [Contact sales](https://bit.ly/SharKtech) |

Dedicated Cloud doesn't have fixed tiers on the public site — you order the allocation you want, and it bills as a flat monthly price for exactly that allocation. For most readers comparing hosted private cloud options, the decision is between Public Cloud (flexibility) and Dedicated Cloud (predictability).

Secondary rates worth knowing before you order (Public Cloud, hourly metering above the included pool):

- CPU: $0.0025 per core/hour
- RAM: $0.0035 per GB/hour
- NVMe storage: $0.000090 per GB/hour; SSD $0.000060; HDD $0.000020
- Additional public IPv4: $1.50/month each (the first is included)
- Bandwidth above included transfer: $0.002/GB

Enterprise gets discounted unit rates (CPU $0.0020/core/hour, RAM $0.0030/GB/hour), which matters once your usage consistently exceeds the base allocation.

## The real cost: where hosted private cloud bills actually explode

The monthly base price is the smallest part of the bill at most providers. Three cost factors decide whether a hosted private cloud is genuinely cheaper than a hyperscaler, and Sharktech publishes all three.

**Egress fees.** Outbound traffic is the classic budget killer. Sharktech charges $0.002/GB above the included transfer and charges nothing for inbound traffic — inbound is free and unlimited. Compare that to the ecosystem around major clouds: AWS Shield Advanced alone starts at $3,000/month, and that's before you count data transfer. On a busy public-facing service, DDoS protection plus egress can exceed the compute bill several times over.

**Metered storage on stopped VMs.** This one surprises people. Per Sharktech's own cost calculator documentation: "Storage stays allocated for the estimated month, even when a VM is stopped." Powering off a VM saves you compute charges, but its disks keep billing until you delete them. If you're running dev/test environments that get shut down nightly, delete the volumes too, or accept the storage line item.

**Resource ceilings are not spending caps.** Sharktech states this explicitly: "Resource ceilings are not monetary spending caps." Small, Medium, and Large plans have hard limits on how much CPU, RAM, and storage you can add, which prevents runaway scaling — but within those limits, you can still accumulate metered charges. Only the Enterprise tier has no preset ceiling.

If you want the bill before the bill, the honest move is to model your exact configuration:

👉 [Open Sharktech's cloud cost calculator and price your actual VM count, storage, and traffic](https://bit.ly/SharKtech)

For longer billing cycles, third-party plan tracking reports 5–15% discounts on the fixed-price Public Cloud tiers for quarterly, semi-annual, and annual billing. Confirm the discount in the cart before committing, since promotions change.

## DDoS protection: the included feature that changes the comparison

Every Sharktech cloud plan and every IP includes 60Gbps DDoS protection at no extra cost — per IP, not a shared pool — with paid upgrades available up to 1Tbps. The protection runs always-on, with layered mitigation at the network, transport, and application levels.

This matters more than it sounds. When you price a hosted private cloud against a commodity VPS, the VPS looks cheaper until it gets knocked offline. Dedicated DDoS protection is expensive technology for any provider to run, which is why most budget hosts either skip it or blackhole your IP under attack. Sharktech's whole identity is built around not doing that; a gaming server operator cited in third-party coverage reported surviving recurring 3–8Gbps attacks without downtime. Backing that up, the network runs on a redundant 40G/100G backbone with direct peering at Any2 (Los Angeles), AMSIX (Amsterdam), and DE-CIX (Chicago).

## No lock-in: why the OpenStack foundation matters

Sharktech's cloud runs on OpenStack, with full RESTful APIs across compute (Nova), block and object storage (Cinder and Swift), networking (Neutron), and identity (Keystone). In practice, this means:

- You can upload your own VM images and ISOs through the portal or API, and download them to leave.
- Standard tooling — Terraform, Ansible, Kubernetes — works against native OpenStack APIs.
- The management layer (Virtuozzo Hybrid Infrastructure) is separate from the billing system, which Sharktech notes reduces cross-system attack surface and lets you split billing duties from cloud operations across teams.

The practical consequence: if you're evaluating hosted private cloud providers, an OpenStack-based one lets you test an exit by exporting images before you're deeply invested. Proprietary platforms don't offer that, and it's one of the quiet reasons migration costs from some providers balloon.

Feature-wise, the resource pool supports Linux and Windows VMs, three storage tiers (HDD for cost, SSD for balance, NVMe for speed), private networks, virtual routers with NAT, floating IPs, firewalls, security groups, load balancers, high-availability failover, native VPN bridging for hybrid setups, and IPv6. Linux images update weekly, and VMs deploy in seconds.

## Who hosted private cloud is right for — and who should skip it

Straight talk, based on the verified pricing and policies:

**Good fit:**
- **Teams leaving AWS or Azure with real workloads.** The Enterprise tier at $499/month for 64 cores, 128GB RAM, and 5TB SSD is hard to beat once you factor in included DDoS protection, $0.002/GB egress, and no proprietary lock-in.
- **Anything that gets attacked.** Game servers, VPN/proxy services, fintech — if your service has a target on it, included 60Gbps-per-IP mitigation is the deciding feature.
- **Hybrid setups.** The free built-in VPN bridges the cloud to your local infrastructure, which is handy for offsite home labs and dev/staging environments.

**Poor fit:**
- **Small websites and low-traffic apps.** A $7.95 Smart VPS (or $3.98/month on annual billing) covers that; paying $39/month for OpenStack primitives you won't use is wasted money.
- **Anyone who might need a refund.** Third-party reviews of Sharktech's terms report **no refunds under any circumstances**. Combined with hourly metering, that means you should configure carefully and confirm charges in the customer portal before your first cycle closes. Payment covers credit card, PayPal, cryptocurrency, and Alipay.
- **Buyers who need mainland-China-optimized routing.** Measured latency from Los Angeles to China Telecom runs about 140–180ms — decent for the US West, but not CN2 GIA territory. Amsterdam sits at 250–300ms to China.

For context against other private cloud options: Bluehost recently launched an OCI-based managed private cloud with one all-inclusive price and no egress fees, which is worth a quote if you want a big-brand relationship. Vultr and DigitalOcean are cheaper for commodity workloads but offer far weaker DDoS protection. OVH is priced similarly to Sharktech with more global locations, though reviewers consistently rate its support lower. None of these change the core math above — but always compare total cost including egress and protection, not base price alone.

## FAQ

**What's the minimum commitment for a hosted private cloud at Sharktech?**
$39/month on the Public Cloud Small plan, billed monthly with hourly metering above the included allocation. If you want to test the infrastructure with less money down, the Smart VPS Tiny plan at $7.95/month (or $3.98/month effective on annual billing) runs on the same DDoS-protected network.

**Public Cloud or Dedicated Cloud — which one?**
If your resource demand swings month to month, Public Cloud's metered model usually costs less, because you only pay for bursts when they happen. If your load is steady and you want a bill that never surprises you, Dedicated Cloud's fixed allocation wins — no hourly charges and no automatic overages; you upgrade through the billing platform when you need more.

**Can I run Windows?**
Yes. Both Linux and Windows VMs are supported, and you can also upload your own licensed images or ISOs through the portal or API.

**Where are the data centers?**
Five locations: Los Angeles, Las Vegas, Denver, Chicago, and Amsterdam. Deploy close to your users to cut latency; US East/Central locations measured 180–220ms to China, and the company maintains direct China Telecom and China Mobile routes via its own AS46844 network.

**What happens if a VM crashes?**
The platform supports high-availability automatic failover, snapshots, and self-service resource management. Support is 24/7 across all plans.

## Final picks, and one thing to check before you click buy

- Want to test OpenStack or run a small stack: start with **Public Cloud Small at $39/month** and scale inside the ceiling.
- Steady workload, predictable budget, zero tolerance for metered surprises: request a **Dedicated Cloud** allocation.
- Migrating a serious workload off a hyperscaler: **Enterprise at $499/month** is where the egress and DDoS savings compound fastest.
- Just hosting a site: honestly, don't buy a private cloud — take the $7.95 VPS and be done.

One pre-order check: since Sharktech doesn't do refunds, log into the customer portal and confirm your exact configuration and estimated first bill before submitting — especially extra IPv4s and storage tiers, which are the two line items people add without noticing. 👉 [Start with Sharktech's Public Cloud plans and lock in your configuration](https://bit.ly/SharKtech)
