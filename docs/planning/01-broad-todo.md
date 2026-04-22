# Broad ToDo

This outline is the high-level sequence for designing and building the Family Book Tracker. It is intended to stay stable and serve as a reference as the project evolves.

This file stays intentionally broad. For delivery phases and sequencing detail, refer to [05-roadmap.md](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/05-roadmap.md). For agent-executable implementation sequencing, refer to [07-ai-agent-execution-plan.md](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/07-ai-agent-execution-plan.md).

## Core Steps

1. [x] Define product scope, users, and core workflows.
2. [x] Decide what belongs in the mobile app versus the web app.
3. [x] Define the shared data model for books, ownership, reading state, reviews, wish-lists, and loans.
4. [x] Define household membership, roles, and Google-based authentication.
5. [x] Define the book lookup strategy, with barcode first and title/author fallback.
6. [x] Design the primary user flows and screens for mobile and web.
7. [x] Define import/export and bulk capture workflows.
8. [x] Design backend APIs, database structure, and permissions.
9. [x] Choose the technology stack and hosting approach.
10. [x] Estimate infrastructure cost with a near-zero-cost bias.
11. [x] Break delivery into phases and produce an implementation roadmap.
12. [x] Define required infrastructure accounts, credentials, environments, and setup prerequisites.
13. [x] Produce an AI-agent-ready execution plan with explicit work packages and hand-off rules.
14. [ ] Complete the infrastructure setup record with real account, project, URL, and client ID values.
15. [ ] Prepare actual external infrastructure: repo, hosting, database, Google Cloud, Google Books API, and OAuth clients.
16. [ ] Populate local and hosted secrets and verify environment naming is consistent.
17. [ ] Confirm implementation prerequisites are complete and hand off to coding agents.
18. [ ] Implement the system in phased work packages.
19. [ ] Validate, stabilize, and prepare for private family launch.

## Planned Follow-On Documents

- [x] Product spec
- [x] Technical design
- [x] Delivery roadmap
- [x] Infrastructure setup guide
- [x] Implementation work breakdown

## Current Focus

1. [ ] Fill in [setup-record.md](/Users/louis/_PERSONAL/04-prototypes/family-books-app/infra/env/setup-record.md).
2. [ ] Complete the account and credential steps in [08-infrastructure-preparation-checklist.md](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/08-infrastructure-preparation-checklist.md).
3. [ ] Start implementation using [07-ai-agent-execution-plan.md](/Users/louis/_PERSONAL/04-prototypes/family-books-app/docs/planning/07-ai-agent-execution-plan.md) once prerequisites are satisfied.
