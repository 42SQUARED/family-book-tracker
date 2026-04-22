# Infrastructure Budget

## Purpose

This document estimates the ongoing infrastructure cost of running the MVP Family Book Tracker. It covers hosting and operational platform costs only.

It does not include:

- engineering time
- design time
- app store fees
- domain registration
- third-party paid support
- one-time setup work

Pricing references were checked on April 22, 2026.

## MVP Infrastructure Assumptions

This budget assumes the approved MVP technical design:

- `apps/web` deployed separately from `apps/api`
- web and API hosted on Vercel
- PostgreSQL hosted on Neon
- Google Sign-In for authentication
- Google Books API used for metadata lookup
- a very small household-scale user base in MVP

Important context:

- this is a private family app with no commercial application for now
- keeping infrastructure costs as close to `$0` as reasonably possible is an explicit goal
- if the product is later monetized or opened to many households, the infrastructure costing must be reconsidered

Expected MVP usage pattern:

- 4 household members
- low concurrent usage
- mostly read-heavy activity
- occasional mobile scans and search lookups
- occasional CSV import/export from web admin

## Recommended Cost Scenarios

### 1. Lean Prototype

Use this if the system is only being tested privately and short interruptions are acceptable.

Estimated monthly infrastructure cost:

- Vercel Hobby: `$0`
- Neon Free: `$0`
- Google Books API: `$0` assumed
- Sentry Developer: `$0` optional

Estimated total:

- `$0/month`

Risk profile:

- fine for testing
- not the right production stance if the app becomes relied on daily

### 2. Practical MVP Production

Use this if the app is live for the household and should be reasonably dependable without over-engineering.

Estimated monthly infrastructure cost:

- Vercel Pro: `$20/month`
- Neon Launch: about `$15/month` typical spend
- Google Books API: `$0` assumed
- Sentry Developer: `$0` optional

Estimated total:

- about `$35/month`

Estimated annual total:

- about `$420/year`

Why this is a safer paid baseline:

- Vercel Pro removes the hobby-tier limitations and includes usage credit
- Neon Launch is the smallest paid database tier that fits a real production posture better than free
- the workload for one household should remain well within a low-cost range

### 3. MVP Production With Paid Monitoring

Use this if you want a cleaner operational setup with hosted error monitoring from day one.

Estimated monthly infrastructure cost:

- Vercel Pro: `$20/month`
- Neon Launch: about `$15/month`
- Sentry Team: `$26/month`
- Google Books API: `$0` assumed

Estimated total:

- about `$61/month`

Estimated annual total:

- about `$732/year`

This is a safer operating posture if you want:

- mobile crash visibility
- backend error tracking
- centralized production troubleshooting

## Line Item Notes

### Vercel

Planned use:

- host `apps/web`
- host `apps/api`

Current official pricing used for estimate:

- Hobby: free
- Pro: `$20/month`, with `$20` included usage credit

Why the estimate is conservative:

- your MVP traffic is extremely small
- one household is unlikely to exceed included allowances unless the app changes shape substantially

Main overage risk areas:

- serverless/function execution
- bandwidth
- image optimization if heavily used later

### Neon

Planned use:

- primary PostgreSQL database

Current official pricing used for estimate:

- Free: `$0`
- Launch: usage-based, with a listed typical spend of `$15/month`

Why the estimate is conservative:

- the dataset size should be small in MVP
- query volume should remain very low for a single household

Main overage risk areas:

- significantly higher data volume
- always-on heavier compute usage
- future expansion to many households

### Google Books API

Planned use:

- metadata lookup by ISBN
- metadata lookup by title/author search

Budget assumption:

- `$0/month` for MVP

Important note:

- Google Books API documentation clearly describes usage and quota handling, but I did not find a simple public per-request price schedule on the official Books API pages used here
- for this budget, I am treating it as zero-cost at MVP scale and assuming usage remains within default quota limits
- this should be rechecked in Google Cloud Console before production launch

### Sentry

Planned use:

- optional error monitoring for web, API, and mobile

Current official pricing used for estimate:

- Developer: free
- Team: `$26/month` when billed annually

Budget recommendation:

- optional for MVP if cost minimization matters
- worth adding early if you want visibility into mobile and backend issues

## Monthly Budget Summary

| Scenario | Estimated Monthly Cost | Estimated Annual Cost |
| --- | ---: | ---: |
| Lean prototype | `$0` | `$0` |
| Practical MVP production | about `$35` | about `$420` |
| MVP production with paid monitoring | about `$61` | about `$732` |

## Recommended Budget

Recommended planning number for lowest-cost launch:

- start with the lean prototype posture and target `about $0/month`

Recommended upgrade point:

- move to the practical paid baseline only if free-tier constraints become annoying in real family use

Recommended planning number for the first paid step-up:

- budget for `about $35/month`

Recommended planning number if you want paid monitoring from the start:

- budget for `about $61/month`

## Budget Caveats

- Vercel and Neon both have usage-based elements, so these are estimates rather than hard ceilings
- if the product stays limited to one household, the costs should remain close to the baseline estimates
- if you later support many households, copy-level inventory, more media storage, analytics, or notification systems, this budget will need to be revised
- if the app becomes commercial, pricing and architecture should be re-evaluated before launch

## Sources

- [Vercel Pricing](https://vercel.com/pricing)
- [Vercel Pro Plan](https://vercel.com/docs/plans/pro)
- [Neon Pricing](https://neon.com/pricing)
- [Google Books API Overview](https://developers.google.com/books/docs/overview)
- [Google Books API Usage Guide](https://developers.google.com/books/docs/v1/using)
- [Sentry Pricing](https://sentry.io/pricing/)
