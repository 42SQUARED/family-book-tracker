# AI-Agent Execution Plan

## Purpose

This document turns the roadmap into an execution plan that AI agents can follow with minimal ambiguity. It defines prerequisites, work packages, expected outputs, and sequencing.

## Agent Operating Assumptions

- agents will work from this repository
- agents should not guess missing infrastructure details
- agents may implement code only after prerequisites are satisfied
- agents should prefer incremental, mergeable changes
- agents should assume low-cost infrastructure is a hard constraint unless explicitly changed

## Required Inputs Before Coding Starts

The following must be available to the agent:

- repository URL and default branch
- chosen package manager
- confirmed stack decisions from the technical design
- infrastructure setup record with all accounts and secret names
- environment names and URLs
- confirmed auth provider details

If any of these are missing, the correct next action is to create or update infrastructure documentation rather than guessing.

## Execution Order

### Work Package 1: Bootstrap Repository

Objective:

- create the monorepo skeleton and shared tooling

Inputs:

- repository access
- package manager choice

Outputs:

- `apps/api`
- `apps/mobile`
- `apps/web`
- shared package folders
- linting, formatting, and workspace configuration
- `.env.example` files

Definition of done:

- fresh clone installs successfully
- all workspaces build or start in placeholder mode

### Work Package 2: Database And Shared Models

Objective:

- create the persistent schema foundation

Inputs:

- database connection details
- approved domain model

Outputs:

- Prisma schema
- initial migration
- seed script for household and admin
- shared types for core entities

Definition of done:

- database can be created from migrations
- seed process succeeds

### Work Package 3: Authentication And Authorization

Objective:

- implement Google Sign-In and household-scoped access control

Inputs:

- OAuth client IDs and secrets
- session secret

Outputs:

- auth flow for web
- auth flow for mobile
- backend session validation
- household membership guard helpers

Definition of done:

- approved household members can sign in
- non-members are denied

### Work Package 4: Book Lookup And Caching

Objective:

- implement book identification and metadata persistence

Inputs:

- Google Books API enabled

Outputs:

- ISBN lookup endpoint
- title/author search endpoint
- Google Books integration module
- local metadata persistence

Definition of done:

- known ISBN can be looked up and cached
- title search returns selectable results

### Work Package 5: Library Core APIs

Objective:

- implement household library records and summary payloads

Outputs:

- add-to-library endpoint
- update library ownership endpoint
- library list endpoint
- book summary endpoint

Definition of done:

- API can answer whether the household owns a title

### Work Package 6: Reading, Reviews, And Loans

Objective:

- implement the rest of the MVP domain flows

Outputs:

- reading status endpoints
- review endpoints
- 5-point rating support
- loan create and return endpoints

Definition of done:

- household summary includes ownership, reading, reviews, want-to-read, and loan state

### Work Package 7: Mobile MVP

Objective:

- implement the primary mobile user flows

Outputs:

- sign-in flow
- scanner flow
- search flow
- summary screen
- quick actions
- bulk intake mode

Definition of done:

- a household member can use the app in a shop or at home to make a book decision quickly

### Work Package 8: Web Admin MVP

Objective:

- implement the admin workflows

Outputs:

- sign-in flow
- library table
- book detail screen
- family member management
- CSV import/export

Definition of done:

- an admin can manage the household library without touching the database directly

### Work Package 9: Hardening And Deployment

Objective:

- make the system reliably deployable and usable

Outputs:

- CI
- deployment documentation
- structured logging
- basic authorization tests
- release checklist

Definition of done:

- a clean deploy can be repeated from documentation

## Hand-off Rules Between Agents

- each work package should produce a clear artifact or merged code change
- agents must document any new environment variables they introduce
- agents must not invent external service configuration
- if a package depends on unavailable credentials, the agent should update docs and stop there
- agents should keep write scope narrow and explicit

## Canonical Documents Agents Must Read

- `docs/planning/02-product-spec.md`
- `docs/planning/03-technical-design.md`
- `docs/planning/04-infrastructure-budget.md`
- `docs/planning/05-roadmap.md`
- `docs/planning/06-infrastructure-setup-guide.md`

## Suggested Next Human Action

Complete the infrastructure setup record and confirm the package manager. After that, agent implementation can begin with Work Package 1.
