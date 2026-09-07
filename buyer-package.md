# BidSignal PK buyer package

This is a self-service packaging sidecar for the existing BidSignal PK public-source feed. It does not change the collector, create an account, attach checkout, send email, or publish a URL.

The package gives a buyer a small, inspectable contract:

```text
supplier-profile.json -> local validation -> local preview -> paid delivery only after activation
```

## Quick preview

From the lane directory:

```bash
python3 -m unittest discover -s buyer-package -p 'test_*.py'
python3 buyer-package/validate_preview.py \
  --profile buyer-package/example-supplier-profile.json \
  --feed public/free-sample.json
```

The command reads local JSON only. It prints profile-specific triage matches and their source URLs; it does not fetch those URLs. Use `--format json` for a machine-readable delivery preview.

## Buyer input

`[supplier-profile.schema.json](supplier-profile.schema.json)` is the versioned contract. A buyer supplies:

- a non-secret `profile_id` and Pakistan supplier name;
- the five existing BidSignal categories, include/exclude terms, brands, regions, preferred agencies, and optional source IDs;
- a plan-shaped delivery preference and Asia/Karachi timezone;
- three explicit acknowledgements: public sources only, verify the original notice, and no bid advice.

The schema is intentionally strict. Passwords, API keys, cookies, tokens, payment data, portal credentials, attachment requests, and arbitrary fields are not profile inputs.

The included profile is an example only. Replace the example supplier name and preferences before using it with a real buyer.

## What the local preview explains

For each candidate it reports:

- `fit_score`: profile fit, with primary category, secondary category, terms, brands, agency, source, and region evidence;
- `buyer_score`: `60% fit + 25% deadline urgency + 15% upstream BidSignal score`;
- `change.state`: `verified-change-history`, `change-candidate`, `new-listing-candidate`, or `no-change-evidence`;
- exact reasons, warnings, action wording, source URL, detail URL, observed time, and fact-vs-inference field lists.

The change label is conservative. The current public sample has no before/after history, so a keyword such as “corrigendum” is only a candidate signal. It is never presented as a verified change. Missing deadlines and stale displayed dates remain warnings.

## Delivery materials

- `[ONBOARDING-DELIVERY.md](ONBOARDING-DELIVERY.md)` — buyer flow, delivery envelope, and operational handoff.
- `[PAID-FREE-BOUNDARY.md](PAID-FREE-BOUNDARY.md)` — exact free sample versus subscription surfaces and activation boundaries.
- `[PUBLIC-SAMPLE-DISTRIBUTION.md](PUBLIC-SAMPLE-DISTRIBUTION.md)` — zero-cost, permission-based sample distribution checklist; current publication is explicitly not run.
- `[sample-post-template.md](sample-post-template.md)` — one useful public post template with an unfilled URL placeholder.
- `[delivery-envelope.example.json](delivery-envelope.example.json)` — example machine-readable result shape.
- `[distribution-manifest.example.json](distribution-manifest.example.json)` — release evidence checklist template.
- `[validate_preview.py](validate_preview.py)` and `[test_validate_preview.py](test_validate_preview.py)` — dependency-free validator, previewer, and tests.

The regression suite keeps semantic cases in [`test-fixtures/preview-feed.json`](test-fixtures/preview-feed.json), so IDs, titles, and ordering from a refreshed public feed cannot break behavior coverage. It also runs a content-agnostic contract check against the current full `public/feed.json` to verify that live output remains valid, deterministic, provenance-linked, and honest about change state.

## Safety and activation boundary

This package is a local contract, not proof of a hosted service. The existing `public/free-sample.json`, dashboard, RSS feed, and full feed remain the source inputs. The buyer must open the originating public notice before acting; BidSignal does not determine eligibility, award probability, legal meaning, or bid content.

Checkout and public distribution are not claimed here. Current workspace facts are recorded in the boundary and distribution documents: Polar supports Pakistan payouts but Google-OAuth onboarding is blocked on an unknown DOB; Lemon Squeezy is blocked on its password/KYC steps; and GitHub repository creation is unavailable. No payment link or public URL is fabricated.
