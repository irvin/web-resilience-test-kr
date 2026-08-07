---
marp: true
lang: en
theme: default
class: invert
paginate: true
size: 16:9
title: "When Submarine Cables Go Dark"
author: "Irvin Chen"
date: "2026-08-07"
---

<style>
section h1,
section h2,
section h3 {
  color: #ff9ed1;
}

section table {
  background: rgb(255 255 255 / 0.055);
  box-shadow: inset 0 0 0 1px rgb(255 255 255 / 0.08);
}

section th,
section td {
  border-color: #686868;
}

section th {
  background: rgb(255 255 255 / 0.1);
  color: #f3f3f3;
}

section td {
  background: rgb(255 255 255 / 0.025);
}

section.cols-slide {
  display: grid;
  grid-template-columns: 42% 1fr;
  grid-template-rows: auto minmax(0, 1fr);
  column-gap: 1.5rem;
  align-items: center;
}

section.cols-slide h1,
section.cols-slide h2,
section.cols-slide h3 {
  grid-column: 1 / -1;
}

section.cols-slide > p:has(> img:only-child) {
  grid-column: 1;
  grid-row: 2;
  margin: 0;
}

section.cols-slide > p:has(> img:only-child) > img {
  display: block;
  width: 100%;
  max-height: 100%;
  object-fit: contain;
}

section.cols-slide > table {
  grid-column: 2;
  grid-row: 2;
  align-self: center;
}

section.tables-slide {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto auto minmax(0, 1fr);
  column-gap: 1rem;
  align-items: start;
}

section.tables-slide h1,
section.tables-slide h2,
section.tables-slide h3 {
  grid-column: 1 / -1;
}

section.tables-slide > table {
  width: 100%;
  font-size: 0.72em;
}

section.tables-slide > table:nth-of-type(1) {
  grid-column: 1;
  grid-row: 2;
}

section.tables-slide > table:nth-of-type(2) {
  grid-column: 2;
  grid-row: 2;
}

section.tables-slide > ul {
  grid-column: 1 / -1;
  grid-row: 3;
  margin-top: 0.75rem;
}

section.rtt-slide {
  display: grid;
  grid-template-columns: 42% minmax(0, 1fr);
  grid-template-rows: auto minmax(0, 1fr);
  column-gap: 1.25rem;
  align-items: start;
}

section.rtt-slide h2 {
  grid-column: 1 / -1;
}

section.rtt-slide > ul {
  grid-column: 1;
  grid-row: 2;
  margin-top: 0.5rem;
}

section.rtt-slide > p:has(> img:only-child) {
  grid-column: 2;
  grid-row: 2;
  align-self: center;
  margin: 0;
}

section.rtt-slide > p:has(> img:only-child) > img {
  display: block;
  width: 100%;
  max-height: 500px;
  object-fit: contain;
}
</style>

# When Submarine Cables Go Dark

## Understanding Taiwan's digital service resilience

![bg right:38% contain](img/text-report-qr.png)

Irvin Chen  
[https://orcid.org/0009-0002-1059-7130](https://orcid.org/0009-0002-1059-7130)  
Open Culture Foundation  
[ROR: 02csnb181](https://ror.org/02csnb181)  

Report: [`resilience.ocf.tw/web/report`](https://resilience.ocf.tw/web/report/) →

<!--
Hello everyone. I am Irvin Chen from the Open Culture Foundation and the MozTW community.

Today I want to start with a simple question: if Taiwan loses its submarine cables, what still works?

We often talk about cables as infrastructure: how many cables Taiwan has, how many are broken, and how fast they can be repaired. These questions matter. But for users, the question is more direct: can I still open the websites and services I need?

The full report is online at resilience dot ocf dot tw slash web slash report.

-->

---

## Acknowledgments

This work was supported by a grant from the [APNIC Foundation](https://apnic.foundation/) ([ROR: 01y4y6h16](https://ror.org/01y4y6h16)), via the [Information Society Innovation Fund (ISIF Asia)](https://apnic.foundation/home/isifasia/).

![bg left:30% contain APNIC Foundation ISIF Asia](img/apnic-foundation-isif.png)

<!--
Before the results, I want to thank the APNIC Foundation and the ISIF Asia Fund for supporting this work.

-->

---

## What Happens If They Go Dark?

![h:550 contain Taiwan submarine cable status map after 20251227 earthquake](img/map-20260103.png)

<!--
Taiwan is an island society. More than 99 percent of our external traffic depends on submarine cables.

Taiwan currently connects to the global Internet through 14 international submarine cables, with another 10 domestic cables linking its offshore islands.

This map shows the cable status in early January, after earthquakes from December 25, 2025 to January 3, 2026.

Six international cable systems, nearly half of Taiwan's systems, were damaged. Full repairs were not completed until May 2026.
-->

---

![bg right:38% contain](img/smc-peering-tw-2026-03-18-1822.png)

## Cables Are Already Failing

More than **99%** of Taiwan's external traffic depends on submarine cables.

At least one cable is often under repair.

<!--
This chart shows the cable status over the past year.

Cable failures are not rare. Taiwan is often in a state where at least one cable is under repair.

Most of the time, users do not notice because traffic can be rerouted. But if several cables fail at the same time and redundancy is exhausted, critical services such as emergency information, payments, and communications may be disrupted.

This brings us to the question: when international connectivity is severely reduced, which services can people still use?
-->

---

## Overall Result: **89%** Need Attention

![bg right:50% contain Overall result](img/overall-result.en.svg)

| Category          | Sites  | Share |
|-------------------|-------:|------:|
| Foreign-dependent |    856 | 39% |
| Cloud-dependent   |  1,080 | 50% |
| Locally-contained |    243 | 11% |

<!--
We tested websites commonly used in Taiwan. The headline result is this: 88.8 percent of tested sites need attention.

39.3 percent are foreign-dependent. They load at least one resource from outside Taiwan.

49.6 percent are cloud-dependent. They look local in the beginning, but they rely on Taiwan nodes of multi-national cloud providers.

This does not mean they will all fail. It means we should verify them before a real outage.

Next, I will explain what we tested and how we measured these dependencies.
-->

---

## Test Target

Websites commonly used in Taiwan

- Tranco `.tw` domains, Cloudflare Radar, Ahrefs, SimilarWeb, Semrush
- Civic-tech and community sites

Latest dataset:

- **2,507** unique sites
- **2,179** successfully measured (**86.9%**)
- Measurements collected **2026-07-21**

<!--
We test websites commonly used in Taiwan, not only Taiwanese websites. This includes global platforms, e-commerce, news, public services, and community sites that people in Taiwan often use.

Our dataset contains 2,507 unique websites; 2,179, or 86.9 percent, were measured successfully.

The ranking snapshots are from July 20, and measurements were collected on July 21, 2026.

For now, we test only each website's homepage. This is not a full service test, but it gives us a repeatable starting point.
-->

---

## Method

For each site:

1. Load homepage in programmable Chromium
2. Filter requests; deduplicate by hostname within site
3. Resolve geolocation and ASN with IPinfo
4. Detect local cloud edges via headers, LACeS, then RTT
5. Aggregate requests into dependency categories

<!--
For each site, we open the homepage in Chromium and record every network request generated during loading.

We remove requests that are not relevant to core functionality and deduplicate them by hostname. IPinfo then gives us a first estimate of each resource's location and network provider.

Cloud and CDN endpoints are harder because their IP registration may not match where the resource is actually served. For six providers with Taiwan edge nodes, we verify location using response headers, then LACeS, and use RTT only as the final fallback.
-->

---

## RTT Fallback Is Robust

<!-- _class: invert rtt-slide -->

- **RTT fallback:** **3,640 of 19,046** observations (19.1%); **3,064** returned measurements (84.2%)
- **Website impact:** **2,394** RTTs below 15 ms shift **514 sites (23.6%) foreign → cloud**
- **Stable across cutoffs:** vs. 15 ms, 10 ms shifts **27 sites (1.2%) cloud → foreign**; 20 ms shifts **5 (0.2%) foreign → cloud**

![RTT fallback minimum latency distribution](../img/rtt-scatter-plot.en.svg)

<!--
Of 19,046 observations, 3,640 entered RTT fallback, and 3,064 returned measurements.

Using our 15-millisecond cutoff, we identified 514 websites whose observed homepage resources were served from Taiwan cloud endpoints rather than abroad, reclassifying them from foreign-dependent to cloud-dependent.

The overall result remains stable across the 10-, 15-, and 20-millisecond cutoffs.

Finally, we aggregate the resource-level results and classify each website as foreign-dependent, cloud-dependent, or locally-contained.
-->

---

## 11.2% !== Safe

This is a **risk map**, not a live outage simulation.

Even if the homepage looks local -

- Databases and backend APIs
- Login, payment, search, and forms
- Mobile app traffic
- Cloud control planes and authentication
- Actual routing paths

<!--
To recap the overall picture, 39 percent are foreign-dependent, 50 percent are cloud-dependent, and only 11 percent are locally-contained.

But locally-contained only means that all observed homepage resources appear local. It does not prove that the whole service is local.

The database may be outside Taiwan. The API may be outside Taiwan. Login, payment, search, forms, and mobile apps may use different paths.

So this is a risk map. It is not a live outage simulation, and it is not a final pass.
-->
---

## Local Cloud/CDN Concentration

Among **1,323** sites without observed foreign resources:

| Provider Taiwan nodes | Sites | Share |
|-----------------------|------:|------:|
| Google                |   965 | 72.9% |
| Cloudflare            |   480 | 36.3% |
| Amazon                |   138 | 10.4% |
| Akamai                |   104 |  7.9% |
| Microsoft             |    38 |  2.9% |
| Fastly                |     4 |  0.3% |

Can local edges keep serving when Taiwan is isolated?

<!--
Now, let us look more closely at cloud-dependent sites. We observed no foreign requests from these sites, but they still rely on Taiwan nodes operated by multinational cloud and CDN providers.

This reliance is highly concentrated. Google appears on 965 sites, or 72.9 percent, and Cloudflare on 480, followed by Amazon, Akamai, Microsoft, and Fastly. Because one site may use several providers, these counts overlap.

Local nodes are valuable: they reduce latency and keep resources close to users.

But a local node does not mean a service can operate independently. During an international outage, it may still depend on overseas origins, control planes, authentication, certificates, caches, and provider operations.

So the key question is: can these apparently localized services keep running if Taiwan is isolated?
-->

---

## Cloud Resource Exposure

<!-- _class: invert tables-slide -->

| Sites                      | Domestic | Foreign |  Total |
| -------------------------- | -------: | ------: | -----: |
| Multinational public cloud |    1,881 |     754 |  1,910 |
| Non-cloud                  |    1,623 |     245 |  1,709 |
| Total                      |    2,140 |     856 |        |

| Provider   | Sites (domestic nodes) | Sites (foreign nodes) |
| ---------- | ---------------------: | --------------------: |
| Google     |                  1,685 |                    56 |
| Cloudflare |                  1,016 |                    17 |
| Amazon     |                    512 |                   309 |
| Akamai     |                    338 |                    11 |
| Fastly     |                      6 |                   257 |
| Microsoft  |                    140 |                    77 |

- **87.7%** use multinational public-cloud resources
  - 86.3% use domestic cloud endpoints
  - 34.6% use foreign cloud endpoints
- Only **39 sites (1.8%)** use foreign resources exclusively

<!--
To see the broader picture beyond local cloud nodes, we examined cloud usage across all measured websites, including both domestic and foreign endpoints. These tables show two important points.

First, global-cloud use is widespread: 87.7 percent use at least one resource from a multinational cloud or CDN. The domestic and foreign figures overlap because a site can use both.

Localization already helps. Only 39 sites, or 1.8 percent, use foreign resources exclusively. Many websites, including foreign services, already serve resources from local.

Second, provider behavior is very different. Some providers serve most observed resources from Taiwan nodes. Others have a much more mixed pattern.

So the answer is not simply "avoid cloud" or "host everything locally." CDNs and local cloud nodes can improve resilience, but we need to test how each provider behaves during isolation.
-->

---

## Resource Distribution

Among **18,969** observations with provider data, dependency is **highly concentrated**.

![h:450 Resource source distribution](img/resource-distribution.en.svg)

<!--
Now we look at all resources loaded by the tested website homepages.

When we group them by ASN, the pattern is very concentrated. A small number of providers account for a large share of the observed resources.

This is how the modern web works. It is not evenly spread across many operators. It depends heavily on a few infrastructure and platform providers.

That can help if those providers localize resources well. But it also means resilience depends on a small number of third-party systems.
-->

---

## Public Sector

Foreign dependency is lower in government and education sites.

| Type | Sites tested | Foreign dependencies | Share |
| --- | ---: | ---: | ---: |
| Government | 235 | 16 | 6.8% |
| Education | 255 | 34 | 13.3% |
| All sites | 2,179 | 856 | 39.3% |

<!--
There is one positive finding.

Government and education sites have lower foreign dependency than the overall dataset.

Only 6.8 percent of government sites had observed foreign dependency, compared with 39.3 percent overall. Education sites were also below the average.

This does not prove they are fully safe. But it suggests a better starting point in public-sector and academic-network environments.
-->

---

## Policy Recommendations

- Continuously monitor critical and commonly used services
- Validate major cloud Taiwan-node outage behavior
- Reduce foreign-resource dependency in critical services
- Require backup plans and disconnection exercises
- Define resilience tiers for procurement
- Plan bandwidth priority before the emergency

<!--
For policy, I want to highlight three directions.

First, monitor critical and commonly used services continuously. We need both the big picture and per-service indicators.

Second, test major cloud Taiwan nodes before an outage. Many services depend on them, and we need the providers to help find out what happens when Taiwan is isolated.

Third, reduce foreign-resource dependency in critical services, and require backup plans and disconnection exercises.

We also need pro-cure-ment rules and bandwidth priority plans before an emergency, because it will be too late to recover if we do not already have backups in place.
-->

---

## For Developers

- Plan for external connectivity failures
- Prefer CDNs with Taiwan-based nodes
- Add local fallbacks for critical libraries
- Localize data for critical paths: login, checkout

<!--
Developers can also act on this.

When you build a website, think about what happens if external connectivity fails.

If you use CDN libraries, prefer services with Taiwan-based nodes, and add local fallbacks for critical files.

For important service paths, such as login and checkout, consider where the data and APIs are located.

If you work on cloud services or infrastructure, this is also a design question worth working on.
-->

---

## Potential Follow-up Research

- Fault injection for foreign-resource failure
- Journey tests: login, transactions, browsing, search
- Cloud resilience: control planes, origins, cache, auth
- Traceroute, library, sector, and resource-type analysis
- Identify high-traffic, low-resilience sites & resources

## What about apps?

- OCF is developing a mobile app testing service

<!--
This study is a first structural map. The next step is to test behavior more directly.

We can run fault injection and block foreign resources. We can test real user journeys, such as login, transactions, browsing, and search.

We also need deeper cloud resilience tests: control planes, origins, cache, authentication.

And of course, many people use mobile apps more than websites. OCF is developing a mobile app testing service. Stay tuned later this year.
-->

---

![bg right:38% contain](img/report-qr-footer.png)

## Participating

Check your website at [`resilience.ocf.tw`](https://resilience.ocf.tw/) →

- g0v Digital Resilience Hackathon  

- Meet us at APAN62 on Aug 12 in Auckland

## Channels *海纜又被鯊魚咬斷了*

- Telegram: [`t.me/s/smc_resilience`](https://t.me/s/smc_resilience)
- Fediverse: [`g0v.social/@smc_resilience`](https://g0v.social/@smc_resilience)
- FB . IG . Threads: [`@smc.resilience`](https://facebook.com/smc.resilience)

## Contact

`t.me/irvin` . `irvin @ moztw.org` . `@irvin`

<!--
The results are published at resilience dot ocf dot tw. You can look up your sites, read the report, and fork the source code.

Our goal is to make resilience visible enough that we can improve it.

This work started from g0v digital resilience hackathons, and we will continue the follow-up work there.

We will also be at APAN62 in Auckland in August. If you will be there, please come talk to us.

Please follow our channels for project updates and Taiwan submarine cable news.
-->
