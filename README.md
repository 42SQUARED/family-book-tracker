# Family Book Tracker

Family Book Tracker is a private household system for tracking books, reading activity, reviews, wish-to-read status, and loans across a family.

The planned product includes:

- a mobile app for barcode scanning, quick lookup, and day-to-day updates
- a web app for admin tasks such as imports, exports, member management, and library maintenance
- a shared backend, database, and authentication model used by both clients

This repository currently contains the planning and setup documentation needed to prepare the infrastructure and support AI-agent-led implementation.

## Project Goals

The system should let a family member quickly answer:

- Do we own this book?
- Have I read it?
- Has anyone else in the family read it?
- Has anyone left a review?
- Does anyone want to read it?
- Is it currently loaned out?

## Current Product Direction

The approved MVP direction is:

- Google Sign-In for authentication
- Google Books API for metadata lookup
- title-level ownership in MVP, not copy-level ownership
- shared household reviews
- no offline-first requirement for launch
- low-cost infrastructure with a strong preference for staying near `$0` while the app is private and non-commercial

If the app is later monetized or scaled beyond private family use, both the infrastructure design and cost model must be revisited.

## Project Structure

```text
docs/
  planning/
    01-broad-todo.md
    02-product-spec.md
    03-technical-design.md
    04-infrastructure-budget.md
    05-roadmap.md
    06-infrastructure-setup-guide.md
    07-ai-agent-execution-plan.md
    08-infrastructure-preparation-checklist.md
infra/
  env/
    setup-record.md
    variable-inventory.md
```

## Planning Documents

- [Broad ToDo](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/01-broad-todo.md)
- [Product Spec](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/02-product-spec.md)
- [Technical Design](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/03-technical-design.md)
- [Infrastructure Budget](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/04-infrastructure-budget.md)
- [Roadmap](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/05-roadmap.md)
- [Infrastructure Setup Guide](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/06-infrastructure-setup-guide.md)
- [AI-Agent Execution Plan](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/07-ai-agent-execution-plan.md)
- [Infrastructure Preparation Checklist](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/08-infrastructure-preparation-checklist.md)

## Infrastructure Working Files

- [Setup Record](/Users/louis/_PERSONAL/04-prototypes/family-books-app/infra/env/setup-record.md)
- [Variable Inventory](/Users/louis/_PERSONAL/04-prototypes/family-books-app/infra/env/variable-inventory.md)

## Secret Handling

This repository should contain configuration structure and placeholder examples, but not live secrets.

Do not commit:

- `.env` or `.env.local` files with real values
- database URLs
- Google OAuth client secrets
- session secrets
- downloaded credential files such as `client_secret.json`
- private keys or service-account files

Safe to commit:

- `.env.example` files with placeholder values
- variable names and documentation
- public client IDs when needed

See [Infrastructure Setup Guide](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/06-infrastructure-setup-guide.md) and [Variable Inventory](/Users/louis/_PERSONAL/04-prototypes/family-books-app/infra/env/variable-inventory.md) for the working rules.

## Current Status

Completed:

- broad planning
- product specification
- technical design
- cost estimation
- delivery roadmap
- infrastructure planning
- AI-agent execution planning

Current phase:

- complete the real infrastructure setup record
- create the required external accounts and credentials
- prepare the repo, hosting, database, Google Cloud project, Google Books API, and OAuth clients
- then begin implementation using the agent execution plan

## Recommended Next Steps

1. Fill in [setup-record.md](/Users/louis/_PERSONAL/04-prototypes/family-books-app/infra/env/setup-record.md) with actual account, project, URL, and client ID values.
2. Follow [08-infrastructure-preparation-checklist.md](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/08-infrastructure-preparation-checklist.md) to prepare GitHub, Vercel, Neon, and Google Cloud.
3. Once prerequisites are complete, start implementation from [07-ai-agent-execution-plan.md](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/07-ai-agent-execution-plan.md).

## Cost Posture

This project is currently intended as a private, non-commercial family app. Infrastructure choices should stay as close to `$0` as reasonably possible unless real usage forces an upgrade.

See [Infrastructure Budget](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/04-infrastructure-budget.md) for the current estimate and upgrade posture.
