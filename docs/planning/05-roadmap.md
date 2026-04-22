# Roadmap

## Purpose

This roadmap breaks the approved product and technical design into practical delivery phases. It is optimized for a private family MVP, low operational cost, and steady progress toward working software.

## Planning Principles

- prioritize working end-to-end flows over breadth
- keep infrastructure as close to `$0` as possible at first
- deliver the mobile barcode check flow early
- keep admin workflows simple until real data exposes pain points
- defer scale-oriented complexity unless the product becomes commercial
- make prerequisites explicit so AI agents can execute work with minimal ambiguity
- provision infrastructure and secrets before assigning implementation work to agents

## Delivery Phases

### Phase 0.1: Accounts, Access, And External Services

Goal:

- create and document the external infrastructure and credentials needed before implementation starts

Expected outcomes:

- source code repository created
- repository access permissions defined
- deployment accounts created for hosting and database
- Google Cloud project created
- Google Sign-In credentials created for web and mobile
- Google Books API enabled
- environment variable inventory defined
- secret storage approach chosen
- development, preview, and production environments named

Required outputs:

- repository URL
- deployment account owners
- database project name
- Google Cloud project ID
- OAuth client IDs by platform
- documented list of required secrets and where they are stored

Exit criteria:

- all required third-party accounts exist
- all required credentials have been created
- an AI agent can be given the setup inventory and proceed without guessing missing infrastructure details

### Phase 0.2: Foundation And Repo Setup

Goal:

- create the codebase structure and developer foundation so implementation can proceed cleanly

Expected outcomes:

- monorepo initialized with `apps/api`, `apps/mobile`, and `apps/web`
- shared TypeScript configuration in place
- shared package structure in place for `db`, `types`, and `api-client`
- environment variable strategy documented
- environment templates checked into the repo
- formatting, linting, and basic CI established

Exit criteria:

- all apps boot locally
- shared packages build successfully
- basic CI passes
- no undocumented environment variables remain

### Phase 1: Database, Auth, And Household Access

Goal:

- establish the minimum secure backend foundation

Expected outcomes:

- PostgreSQL connected
- Prisma schema created for core entities
- Google Sign-In integrated
- application session flow working for web and mobile
- household membership checks enforced
- initial admin household seeded manually for development
- secret loading works consistently in local and hosted environments

Exit criteria:

- each family member can sign in
- non-members cannot access household data
- authenticated user can fetch `me` and household context

### Phase 2: Book Lookup And Library Core

Goal:

- make the system able to identify books and persist household library records

Expected outcomes:

- Google Books lookup by ISBN implemented
- title/author search implemented
- local caching of fetched book metadata implemented
- household library CRUD working
- title-level ownership model implemented
- summary endpoint for a single book working

Exit criteria:

- a scanned or searched book can be found and added to the household library
- repeated lookups use cached data when available
- the app can answer whether the household owns the title

### Phase 3: Reading Status, Reviews, And Loans

Goal:

- complete the core family tracking value of the product

Expected outcomes:

- per-member reading status implemented
- `want_to_read` implemented through reading status model
- shared reviews and 5-point ratings implemented
- title-level loan tracking implemented
- family summary payload includes read, want-to-read, review, and active loan information

Exit criteria:

- a family member can mark a book as read or want to read
- a family member can leave a shared review and 5-point rating
- a book can be marked as loaned out and later returned

### Phase 4: Mobile MVP

Goal:

- deliver the primary day-to-day user experience on iOS and Android

Expected outcomes:

- sign-in flow working on mobile
- barcode scanning flow working
- search fallback working
- book summary screen working
- quick actions for owned, read status, want to read, review, and loan state working
- bulk intake mode working at a basic level
- library browse and filter screen working

Exit criteria:

- while in a shop or at home, a user can scan a book and get a useful answer in seconds
- users can update their own reading state from mobile
- users can browse the family library on mobile

### Phase 5: Web Admin MVP

Goal:

- deliver the supporting admin workflows needed to manage the household library

Expected outcomes:

- web sign-in flow working
- library table with filtering and sorting working
- book detail page working
- member management screen working
- CSV import working
- CSV export working
- basic metadata correction flow working

Exit criteria:

- an admin can import a starter library
- an admin can export the current library
- an admin can manage family members and inspect books from the web app

### Phase 6: Stabilization And Launch Readiness

Goal:

- harden the MVP for reliable real household use

Expected outcomes:

- error handling improved across mobile, web, and API
- structured logging enabled
- optional monitoring added if needed
- basic authorization and cross-household access tests added
- performance checks for scan-to-result latency completed
- deployment process documented
- operational runbook documented for future AI agents or humans

Exit criteria:

- the app is usable by the household without blocking defects
- core flows are tested
- production deployment is repeatable

## Suggested Milestone Sequence

### Milestone 1: Backend Skeleton

Includes:

- Phase 0.1
- Phase 0.2
- the schema and auth parts of Phase 1

Target result:

- secure foundation and infrastructure in place, no real product features yet

### Milestone 2: Library Core Working

Includes:

- remaining Phase 1
- Phase 2
- backend parts of Phase 3

Target result:

- the backend can identify books and answer core ownership and reading questions

### Milestone 3: Mobile First Usable MVP

Includes:

- Phase 4
- enough of Phase 3 surfaced in UI

Target result:

- family can start using the app in real life from mobile

### Milestone 4: Admin Completion

Includes:

- Phase 5

Target result:

- web admin fills the management gaps left by the mobile-first MVP

### Milestone 5: Launch Hardening

Includes:

- Phase 6

Target result:

- stable private-family launch

## Out Of Scope Until After MVP

- copy-level ownership
- shelf or location tracking
- offline-first mobile sync
- notifications and reminders
- recommendations
- multi-household productization
- billing, monetization, or commercial-grade scaling

## Cost Strategy By Phase

### Infrastructure setup

- prefer free plans and single-owner administration where possible
- create only the accounts and projects required for MVP
- document all secrets and environment names early so agent work does not stall

### Early phases

- prefer free tiers wherever possible
- keep hosting close to `$0`
- avoid adding paid monitoring unless a real need appears

### Later private-family launch

- move to paid hosting only if reliability or limits force it

### If monetization starts

- revisit hosting, database sizing, monitoring, auth, and operational budget before product launch

## Main Risks To Watch During Delivery

- barcode and ISBN quality may be worse than expected for some books
- Google Books metadata quality may require more admin correction than expected
- mobile auth setup can become fiddly if web and mobile flows diverge too much
- title-level ownership may feel limiting sooner if the household has multiple copies of the same title

## Recommended Next Documents

- infrastructure setup guide
- AI-agent execution plan
- initial backlog and ticket decomposition
