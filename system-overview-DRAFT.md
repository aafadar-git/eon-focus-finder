# EON Focus Finder — System Overview (DRAFT)

*One-page description for internal/stakeholder use. Vendor-neutral. No source names mentioned.*

## What it is

A static web application that ranks a portfolio of **788 companies** by commercial focus priority, using a transparent 0–100 score. Each record carries an enrichment overlay (subvertical, headquarters, revenue, headcount, training footprint, recent expansion, safety signals) and a verified training-leader contact where one is publicly identifiable. An integrated Q&A module answers questions strictly from the embedded dataset via a language-model API with user-supplied credentials.

## Technical architecture

- **Single self-contained page**, served from a content-delivery network. No backend, no database, no user sessions, no server to run.
- All 788 records are embedded as JSON in the page; ranking, filtering, score breakdowns, and Q&A retrieval all execute in the visitor's browser.
- The Q&A module performs client-side keyword retrieval over the embedded records and forwards the top matches to an OpenAI-compatible language-model endpoint. The visitor's API key is stored only in their own browser's local storage — it is never transmitted to, or stored by, the page's publisher.

## Scoring

Deterministic and recomputable from the data at any time:

```
score = tier weight + trigger signals + recency + contact readiness
        + research depth + enrichment − penalty
```

No machine-learning model, no hidden weights. Every company's score can be expanded in the page to show its component parts.

## How the data was populated

- **Enrichment (8 fields per company):** 34 research waves of ~20 companies each, with five autonomous research agents per wave, extracting fields from public sources (company websites, press releases, filings, reputable publications). Where nothing verifiable existed, the field records "not publicly disclosed" — values are never guessed.
- **Contacts / training leads:** targeted research for training & learning leaders only — CLOs, heads of learning, academy directors; for training institutions, the senior-most operator. Names and titles only; no email addresses or phone numbers. Every contact is labeled by evidence tier: company-official source, reputable press, or aggregator (single-aggregator listings marked unconfirmed). Final tally: **148 verified training leaders · 65 weak fallbacks** (HR/operations contacts kept but badged "not the training owner") **· 574 recorded as none found**.
- **Honesty rule:** a blank result on a company with thin public coverage is the correct output, not a gap to fill. No contact names are ever invented.

## Data freshness (planned)

The page is currently a read-only snapshot: new developments require a refresh cycle — re-research the portfolio for new trigger events (project FIDs, M&A, leadership changes, incidents), recompute scores with the same formula, and re-publish the data file. Refresh cadence to be set by the data owner (recommended: monthly for the top ~150 companies by score, quarterly rotation for the remainder). Each release stamps a visible "Data refreshed <date>" header so staleness is never ambiguous.

## Limits

- Read-only snapshot between refreshes; there is no write path and no live data feed.
- Web research finds *news*, which is softer and less uniform than a systematic trigger feed — scores move on real developments, but coverage is not uniform across all 788 companies.
- Because the full dataset ships with the page, anyone with the link can download all 788 records as JSON. Treat the link accordingly.

---
*Draft prepared Oct 18, 2026. Verify counts against the live dataset before external distribution.*
