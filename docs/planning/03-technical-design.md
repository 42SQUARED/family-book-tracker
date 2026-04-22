# Technical Design

## Purpose

This document proposes the MVP technical design for Family Book Tracker. It translates the approved product spec into a system architecture, data model, API shape, authentication model, and deployment approach that can be implemented incrementally.

The design optimizes for:

- Fast mobile scanning and lookups
- Simple household sharing
- Low operational complexity
- A clean path from MVP to later extensions

## MVP Technical Decisions

- One backend serving both mobile and web clients
- One relational database as the source of truth
- Google Sign-In for authentication
- Household membership and authorization managed by the application
- Ownership tracked at title level, not copy level
- Reviews shared within the household
- No offline-first requirement in MVP
- Google Books API used as the initial metadata source
- API deployed as a separate application under `apps/api`
- Ratings use a 5-point scale

## Proposed Stack

## Client Applications

### Mobile

- React Native with Expo
- Shared TypeScript codebase
- Camera and barcode scanning via Expo-supported libraries

Why:

- One codebase for iOS and Android
- Fast MVP delivery
- Good enough native access for barcode scanning
- Strong alignment with a web TypeScript stack

### Web

- Next.js
- TypeScript
- Admin-oriented responsive web UI

Why:

- Good fit for authenticated dashboards and CRUD-heavy screens
- Easy deployment
- Can share validation and API client code with mobile

## Backend

- A separate API application under `apps/api`
- TypeScript service deployed independently from the web frontend
- ORM: Prisma
- API style: JSON over HTTPS

Why:

- Keeps backend concerns separate from the web UI
- Makes mobile and web integration symmetrical
- Leaves room for scaling backend concerns later without re-cutting the repo
- Prisma gives fast schema evolution and type-safe access

## Database

- PostgreSQL

Why:

- Strong relational modelling for households, books, statuses, reviews, and loans
- Good indexing and filtering support for admin workflows
- Straightforward future growth

## Hosting

- Vercel for web and API hosting
- Managed PostgreSQL such as Neon or Supabase Postgres
- Object storage optional later, not required for MVP

Why:

- Low setup burden
- Good fit for a TypeScript web stack
- Fast iteration for MVP

## Repository Structure

Recommended medium-term structure once implementation starts:

```text
docs/
  planning/
    01-broad-todo.md
    02-product-spec.md
    03-technical-design.md

apps/
  api/
  mobile/
  web/

packages/
  api-client/
  auth/
  config/
  db/
  types/
  ui/

infra/
  env/
  scripts/
```

Notes:

- `apps/api` contains the backend API service
- `apps/mobile` contains the Expo app
- `apps/web` contains the Next.js web app
- `packages/db` contains Prisma schema and database access
- `packages/types` contains shared domain and API types
- `packages/api-client` contains typed API wrappers used by mobile and web

## High-Level Architecture

```text
Mobile App (Expo) ----\
                        --> API App --> PostgreSQL
Web App (Next.js) -----/        |
                                --> Google OAuth
                                --> Google Books API
```

System responsibilities:

- Mobile app handles scanning, quick lookup, quick updates, and library browsing
- Web app handles admin workflows, imports/exports, editing, and management screens
- Backend owns authentication, authorization, persistence, lookup orchestration, import/export, and household rules
- PostgreSQL stores all application state
- Google Books API provides external book metadata lookup

## Architecture Style

Use a modular monolith for MVP.

Definition:

- One deployable backend application
- Internally separated by domain modules
- No microservices

Recommended backend modules:

- Auth
- Households
- Members
- Books
- Library
- Reading
- Reviews
- Loans
- Imports
- Exports

Why this is the right shape:

- The domain is not large enough to justify distributed systems
- Most operations need consistent relational queries anyway
- Easier local development and lower hosting complexity

## Core Domain Model

The MVP should model titles, not individual physical copies.

This means:

- A household either owns a title or does not
- A title can have per-member statuses, reviews, and loan state
- Copy-specific problems are deferred until later

## Main Entities

### User

Represents a signed-in identity.

Fields:

- `id`
- `google_sub`
- `email`
- `display_name`
- `avatar_url`
- `created_at`
- `updated_at`

Notes:

- `google_sub` should be the stable identity key from Google
- Email changes should not break account linkage

### Household

Represents one family unit and its shared library.

Fields:

- `id`
- `name`
- `created_at`
- `updated_at`

### HouseholdMember

Connects a `User` to a `Household`.

Fields:

- `id`
- `household_id`
- `user_id`
- `role` with values `admin` or `member`
- `status` with values `active`, `invited`, `disabled`
- `display_name_override`
- `created_at`
- `updated_at`

Notes:

- All permissions should flow through this table
- A user may support multiple households later, even if MVP UI assumes one

### Book

Canonical title-level work stored in the app.

Fields:

- `id`
- `google_books_id`
- `primary_isbn_10`
- `primary_isbn_13`
- `title`
- `subtitle`
- `authors_json`
- `description`
- `publisher`
- `published_date`
- `page_count`
- `language`
- `thumbnail_url`
- `categories_json`
- `raw_source_json`
- `created_at`
- `updated_at`

Notes:

- `raw_source_json` stores the original Google Books payload for traceability
- Authors and categories can stay JSON in MVP to avoid premature normalization

### HouseholdBook

Represents a book known to a household and whether the household owns it.

Fields:

- `id`
- `household_id`
- `book_id`
- `is_owned`
- `format` with values such as `physical`, `ebook`, `audiobook`, `unknown`
- `added_by_member_id`
- `added_at`
- `updated_at`

Constraints:

- Unique on `household_id + book_id`

Notes:

- This is the anchor record for household-level library state
- In MVP, loan tracking attaches to this title-level ownership record

### ReadingStatus

Represents one member's reading state for one household book.

Fields:

- `id`
- `household_book_id`
- `member_id`
- `status` with values `not_started`, `reading`, `read`, `abandoned`, `want_to_read`
- `started_at`
- `finished_at`
- `updated_at`

Constraints:

- Unique on `household_book_id + member_id`

Notes:

- `want_to_read` is stored as part of the same status model for MVP simplicity
- This avoids splitting reading state and wish-list state too early

### Review

Represents a member's review for a household book.

Fields:

- `id`
- `household_book_id`
- `member_id`
- `rating` integer, nullable
- `review_text`
- `created_at`
- `updated_at`

Constraints:

- Unique on `household_book_id + member_id`

Notes:

- One active review per member per book is enough for MVP
- Reviews are visible to all household members
- Ratings use a 5-point scale

### Loan

Represents a current or historical loan event.

Fields:

- `id`
- `household_book_id`
- `borrower_name`
- `loaned_by_member_id`
- `loaned_at`
- `returned_at`
- `notes`
- `created_at`
- `updated_at`

Rules:

- At most one active loan per `household_book_id` in MVP
- Active loan means `returned_at IS NULL`

### ImportJob

Tracks CSV imports from the web admin.

Fields:

- `id`
- `household_id`
- `created_by_member_id`
- `status`
- `source_filename`
- `summary_json`
- `created_at`
- `updated_at`

## Simplified Entity Relationships

```text
User 1---* HouseholdMember *---1 Household
Household 1---* HouseholdBook *---1 Book
HouseholdBook 1---* ReadingStatus
HouseholdBook 1---* Review
HouseholdBook 1---* Loan
HouseholdMember 1---* ReadingStatus
HouseholdMember 1---* Review
HouseholdMember 1---* Loan
```

## Authentication And Authorization

## Authentication

Use Google OAuth / OpenID Connect.

Flow:

1. User signs in with Google from mobile or web.
2. Backend validates Google identity and creates or updates `User`.
3. Backend checks whether that identity belongs to an allowed `HouseholdMember`.
4. Backend creates an application session.

Session strategy:

- Web: secure HTTP-only cookie session
- Mobile: token-based session issued by backend after Google sign-in

Notes:

- Do not authorize purely by email domain
- Membership must be explicit in application data

## Authorization

Authorization should be household-scoped.

Every request that accesses household data must:

- identify the current user
- resolve the active household membership
- enforce role and membership checks

MVP permission model:

- `admin`: full household management, imports/exports, membership changes, metadata correction
- `member`: scan, search, browse, update own statuses, create own review, view shared household data, record loans if allowed by UI policy

Since children are unrestricted in MVP:

- no child-specific permission branch is required in backend logic

## Book Lookup And Matching

## Lookup Sources

- Primary source: Google Books API
- Inputs: ISBN/barcode, title, author

## Lookup Strategy

### Barcode / ISBN

1. Scan barcode on device.
2. Normalize digits.
3. Try exact local lookup by ISBN-13 or ISBN-10 in the database.
4. If not found, query Google Books API by ISBN.
5. Normalize and store result in `Book`.
6. Create `HouseholdBook` only if the household chooses to add or mark owned.

### Title / Author Search

1. User enters search terms.
2. Backend queries local books first for household relevance and speed.
3. Backend queries Google Books API when local results are insufficient.
4. User selects the intended book.
5. Backend persists canonical book metadata if not already stored.

## Matching Rules

- Prefer ISBN exact matches over fuzzy title matches
- Deduplicate by Google Books ID where available
- Fall back to normalized ISBN uniqueness when Google Books ID is missing
- Allow admin correction later if mismatches occur

## Caching Strategy

- Persist Google Books results in the database after first successful lookup
- Reuse cached records for future searches and scans
- Do not refresh metadata automatically in MVP unless an admin explicitly triggers it later

## API Design

Use REST-style JSON endpoints for MVP.

Base path example:

- `/api/v1/...`

## Auth Endpoints

- `POST /api/v1/auth/mobile/google`
- `POST /api/v1/auth/logout`
- `GET /api/v1/me`

## Household And Member Endpoints

- `GET /api/v1/household`
- `GET /api/v1/household/members`
- `POST /api/v1/household/members`
- `PATCH /api/v1/household/members/:memberId`

## Book Lookup Endpoints

- `GET /api/v1/books/lookup?isbn=...`
- `GET /api/v1/books/search?q=...`
- `GET /api/v1/books/:bookId`

## Library Endpoints

- `GET /api/v1/library`
- `POST /api/v1/library`
- `GET /api/v1/library/:householdBookId`
- `PATCH /api/v1/library/:householdBookId`

Typical uses:

- add a book to household library
- mark owned or not owned
- set format

## Reading Endpoints

- `PUT /api/v1/library/:householdBookId/reading-status/me`
- `GET /api/v1/library/:householdBookId/reading-status`

## Review Endpoints

- `PUT /api/v1/library/:householdBookId/review/me`
- `GET /api/v1/library/:householdBookId/reviews`

## Loan Endpoints

- `POST /api/v1/library/:householdBookId/loans`
- `POST /api/v1/library/:householdBookId/loans/:loanId/return`
- `GET /api/v1/loans`

## Import / Export Endpoints

- `POST /api/v1/imports/library-csv`
- `GET /api/v1/imports/:importJobId`
- `GET /api/v1/exports/library-csv`

## Summary Response Shape

For the scan and book detail experience, the backend should provide an aggregated summary payload rather than forcing the mobile client to stitch many calls together.

Example shape:

```json
{
  "book": {
    "id": "book_123",
    "title": "Example Book",
    "authors": ["Author One"],
    "thumbnailUrl": "https://...",
    "isbn13": "978..."
  },
  "householdBook": {
    "id": "hb_123",
    "isOwned": true,
    "format": "physical",
    "activeLoan": {
      "id": "loan_1",
      "borrowerName": "John",
      "loanedAt": "2026-04-22T10:00:00Z"
    }
  },
  "me": {
    "readingStatus": "read",
    "review": {
      "rating": 4,
      "reviewText": "Worth reading."
    }
  },
  "family": {
    "readBy": ["Parent 2", "Child 1"],
    "wantToReadBy": ["Child 2"],
    "reviews": [
      {
        "memberName": "Parent 2",
        "rating": 5,
        "reviewText": "Excellent."
      }
    ]
  }
}
```

## Client Responsibilities

## Mobile App Responsibilities

- Authenticate user
- Request camera permission
- Scan barcodes
- Show fast summary views
- Allow quick mutation actions
- Support bulk intake flow
- Cache a small amount of recent UI state if needed

The mobile app should not:

- Contain business rules for permissions
- Talk directly to Google Books API
- Own the source of truth for household state

## Web App Responsibilities

- Authenticate admin and member users
- Provide richer list, filter, import, export, and edit workflows
- Surface validation and import error reporting
- Use the same backend APIs and shared types where possible

## Bulk Intake Design

The mobile bulk intake flow should optimize for speed.

Flow:

1. User enters bulk intake mode.
2. Camera remains open between scans.
3. Successful lookup shows a compact confirmation card.
4. User can accept default action: add to library and mark owned.
5. App immediately returns to scanner after save.

MVP simplifications:

- No offline queue
- No shelf/location tagging
- No copy count management

## Import / Export Design

## Import

Initial import format:

- CSV upload from web admin

Recommended columns:

- title
- authors
- isbn
- owned
- format

MVP CSV recommendation:

- Required columns: `title`
- Optional columns: `authors`, `isbn`, `owned`, `format`

Notes:

- `authors` should be a single text field for MVP, for example `Author One; Author Two`
- `owned` should accept simple boolean-like values such as `true/false`, `yes/no`, or `1/0`
- `format` should accept `physical`, `ebook`, `audiobook`, or be blank
- Do not include per-member reading status in the first import version

Import approach:

- Parse CSV on backend
- Try exact ISBN match first
- Fall back to title/author lookup if ISBN is missing
- Present import summary with created, matched, skipped, and failed counts

## Export

Support CSV export of:

- household library
- ownership state
- reading statuses
- reviews
- active loans

## State Management

## Client State

- React Query or TanStack Query for server state
- Minimal local state for view interactions only

Why:

- Caching, retries, invalidation, and optimistic updates are useful for this product

## Backend State

- PostgreSQL is the source of truth
- Avoid event sourcing or CQRS in MVP

## Validation

- Zod for request and response validation at API boundaries
- Shared validation schemas where practical

## Security Design

Core requirements:

- All household data access is authenticated
- All API access is authorized per household membership
- Sessions are revocable
- Sensitive tokens are never exposed to the browser unnecessarily

Implementation notes:

- Store Google provider secrets in environment variables
- Use HTTP-only secure cookies for web sessions
- Use short-lived access tokens and refresh strategy for mobile if required by the chosen auth library
- Log audit-relevant actions such as membership changes and imports

## Observability

MVP observability should include:

- Structured application logs
- Error tracking such as Sentry
- Basic API latency monitoring
- Import job status and error capture

Important operational metrics:

- lookup success and failure rate
- scan-to-result latency
- import success rate
- API error rate

## Performance Considerations

Critical user expectation:

- Barcode scan to answer should feel near-instant when the book is already known

Performance tactics:

- Index books by ISBN-10, ISBN-13, and Google Books ID
- Index household books by household and ownership
- Precompute or aggregate summary payloads in efficient queries
- Avoid excessive client round-trips for the book detail screen

## Schema Evolution Path

This design leaves room for later upgrades without breaking MVP structure.

Future extensions:

- move from title-level to copy-level ownership by introducing `BookCopy`
- add shelf/location tables
- add notifications and reminders
- support multiple households per user in UI
- support richer review history
- support offline sync if later required

## Main Technical Risks

## 1. Metadata Quality

Risk:

- Google Books data may be incomplete or inconsistent

Mitigation:

- cache source payloads
- support admin correction in web
- prefer ISBN matches over search-only matches

## 2. Title-Level Ownership Limitations

Risk:

- loan tracking is less precise if multiple copies of the same title exist

Mitigation:

- accept this trade-off for MVP
- leave clean migration path to copy-level modelling later

## 3. Barcode Variability

Risk:

- some barcodes may not cleanly map to usable ISBN data

Mitigation:

- support immediate fallback to title/author search
- log failed scan patterns for later improvement

## 4. Authorization Mistakes

Risk:

- household data leakage would be a serious defect

Mitigation:

- enforce household scoping in every data access path
- centralize authorization helpers
- add tests for cross-household access denial

## Recommended Implementation Order

1. Set up monorepo, shared TypeScript configuration, and environment handling
2. Implement database schema and Prisma models
3. Implement Google authentication and household membership checks
4. Implement book lookup and local caching
5. Implement household library CRUD and summary endpoint
6. Implement reading statuses, reviews, and loans
7. Implement mobile scan/search/detail flows
8. Implement web admin library and member management
9. Implement import/export
10. Add monitoring, polish, and production hardening

## Confirmed Low-Level Decisions

- Authentication uses Google Sign-In
- API lives in a separate `apps/api` application
- First CSV import version uses a simple library schema: `title`, optional `authors`, optional `isbn`, optional `owned`, optional `format`
- Ratings use a 5-point scale

## Recommendation

Proceed with:

- Expo React Native mobile app
- Next.js web app with integrated MVP backend
- PostgreSQL plus Prisma
- Google Sign-In
- Google Books API integration through the backend
- Modular monolith architecture in a TypeScript monorepo

This is the lowest-complexity design that still cleanly supports the planned product.
