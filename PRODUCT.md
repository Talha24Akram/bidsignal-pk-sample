# BidSignal PK

## Concrete product

BidSignal PK is a recurring, source-linked intelligence feed for Pakistani IT resellers, OEM-authorized distributors, network and cybersecurity integrators, software houses, AI vendors, telecom suppliers, and managed-service providers.

It answers one narrow question every day:

> Which public-sector ICT opportunities changed, are still actionable, and deserve a bid-team's attention before the deadline?

The product is a monitor, not a bid-writing service. It never promises eligibility, award probability, legal interpretation, or government affiliation.

## Why this lane

### Observed facts

- The [Federal PPRA / EPADS active-tenders page](https://epms.ppra.gov.pk/public/tenders/active-tenders?keyword=&page=1&tender_type=1) says its public list includes currently open tenders and tenders closed within the last ten days. On 7 September 2026, the indexed page exposed roughly 1,570–1,605 listings across 32–33 pages, including Huawei WAN routers, desktop computers, laptops, computer servers and PABX network work.
- [MoITT's public tender index](https://moitt.gov.pk/Tenders) showed an open RFQ for hardware/software with an 11 September 2026 closing date, plus AI-hub, hardware, information-system and office-equipment notices.
- [NITB's public tender page](https://www.nitb.gov.pk/tender.html) showed an active 27 August 2026 pre-qualification invitation for managed IT and integrated services/consultancy firms.
- [The Ministry of Planning tender index](https://pc.gov.pk/web/tender) displayed 32 open notices and specifically listed supply/installation of IT equipment/networking and procurement of IT equipment, computer hardware and software.
- A broad incumbent already sells Pakistan tender alerts: [TenderAlert.pk lists PKR 3,000/month](https://tenderalert.pk/our-services) for all-category daily updates. [PakistanTender describes broad coverage](https://pakistantender.com/tender-alerts) across federal/provincial portals, newspapers, categories and cities. A global analogue, [TendersAlerts.com lists 550 SAR/month](https://tendersalerts.com/en/plans) for paid detail, exports and notifications.
- [Lemon Squeezy's supported-country documentation](https://docs.lemonsqueezy.com/help/getting-started/supported-countries) explicitly lists Pakistan for bank payouts. Its [getting-paid documentation](https://docs.lemonsqueezy.com/help/getting-started/getting-paid) describes bank/PayPal payouts, twice-monthly payout creation, a 13-day hold and a $50 minimum threshold. This validates a Pakistan-compatible route, but account approval/KYC remains an external step.

### Inference

The gap is not “no tender websites exist.” The gap is a narrower, less noisy product for ICT suppliers: source-linked records, sector-fit scoring, closing-date triage, conservative dedupe, provenance, and eventually change tracking from plan → tender → corrigendum → award. Existing broad aggregators validate willingness to pay, while the public source pages show enough high-value ICT signal to support a focused feed.

## Edition 1 scope

Included source indexes are listed in [`sources.json`](sources.json). The first edition collects only public HTML listing metadata from:

- Federal PPRA/EPADS keyword views for computer, software, network, server, cybersecurity, cloud, telecom, data and information system, across a bounded two-page window per query.
- The first-party Federal PPRA/EPMS public procurement-plan index, with a bounded explicit next-page follow. Plans are early demand signals, not open bids; plan documents are not fetched.
- MoITT tenders.
- NITB tenders.
- Ministry of Planning tenders.

The live path fetches only public HTTP(S) listing pages at a modest cadence. It does not download documents in bulk, defeat bot controls, access paywalled/private pages, or submit bids. Every record keeps `source_url`, `detail_url`, observed time and raw listing text. The plan adapter retains a public document link only as provenance for a buyer to open at the source.

## Product mechanics

1. Collect public HTML and save a timestamped raw snapshot plus fetch log.
2. Parse table/listing metadata through standard-library parsers.
3. Dedupe by normalized tender/plan reference, with a conservative title/agency similarity fallback and retained duplicate provenance.
4. Classify into network/security, software/data/AI, hardware/end-user, telecom/infrastructure and managed services using whole-term evidence instead of substring matches.
5. Score fit, deadline urgency, displayed status, official-source confidence and observed change. A planned record is visibly kept out of the open-bid priority lane.
6. Compare normalized fields with `data/history.json` to label `new`, `updated` or `unchanged`; a source outage becomes a cached, degraded row rather than silently fresh data.
7. Write a richer Markdown issue, JSON/CSV/RSS feeds, a static dashboard, free-sample JSON/Markdown, health status and a dated archive.
8. Run daily via the locked shell script or GitHub Actions schedule.

## Buyer self-service

[`buyer-package/`](buyer-package/) is a credential-free onboarding and delivery contract. A buyer edits a strict supplier profile, validates it locally, and previews the current free feed with explainable category/term/brand/agency/region evidence, exclusions, deadline warnings, provenance and conservative change candidates. The package produces no email, account, payment, document download or bid advice. This makes the first interaction productized and repeatable without turning the lane into consulting.

The lightweight engine also accepts the nested profile with `python3 bidsignal.py sample --profile buyer-package/example-supplier-profile.json`; the sidecar preview remains the authoritative strict validator and delivery-envelope shape.

## Refresh resilience

Each public request has a small transient retry budget and an explicit user agent. Parsed rows are cached per bounded source page. A failed, empty or parser-drifted response can use that cache, and the generated `health.json`, `run.json`, report and item flags expose the degraded state. Generated artifacts are atomically replaced. A clean live run writes `last-good.json` and comparison history; a degraded run does not overwrite the clean last-good snapshot.

## Pricing

| Plan | Price | Included product surface |
|---|---:|---|
| Scout | $19/month | One supplier profile, weekly digest, five matched signals, public-source links, seven-day free archive |
| Operator | $49/month | Daily feed, five profiles, deadline triage, category filters, CSV/JSON/RSS export, 90-day archive |
| Team | $99/month | Fifteen profiles, change/corrigendum tracking, shared workspace feed, one-year archive |
| Multi-sector | $199/month | Fifty profiles, all supported ICT categories, multi-user exports, plan/tender/award watchlist |

These are product subscriptions, not consulting hours or custom research. The price ladder is an inference from observed local and international tender-alert pricing; it should be validated with the free sample and a small number of legitimate inbound conversations.

## Free sample strategy

- Publish five highest-ranked signals weekly in `public/free-sample.json`, `public/free-sample.md`, the RSS feed and the static dashboard. The sample now shows score components, provenance, deadline triage and change labels.
- Keep the sample useful enough to demonstrate provenance but limit history and saved profiles.
- Use one existing owner-authorized static host, profile, permissioned community or feed directory where its current terms permit it; do not cold-spam, scrape contact lists, mass-submit or use paid ads. The distribution checklist and post template are in `buyer-package/`.
- Convert when a buyer needs daily refresh, supplier-specific matching, a deadline archive, or change/corrigendum history.
- The sample issue in [`reports/2026-09-07.md`](reports/2026-09-07.md) is the first proof artifact.

## Payout and activation boundary

The recommended checkout is Lemon Squeezy because its public documentation lists Pakistan bank payouts. No checkout link is fabricated in the landing page. Activation still requires Talha to create/approve the merchant account, complete identity/payout verification, and replace the placeholder checkout destination. Until then, the product remains publish-ready and the public free sample is the acquisition surface.

Polar is a second legitimate payout route that supports Pakistan, but Google-OAuth onboarding is blocked on an unknown date of birth. Lemon Squeezy remains blocked on its local password/KYC steps. GitHub repository creation is unavailable, so zero-cost public distribution is prepared but **NOT RUN**; no public URL, traffic or sale is claimed.
