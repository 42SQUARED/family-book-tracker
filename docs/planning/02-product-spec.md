# Product Spec

## Product Name

Family Book Tracker

## Summary

Family Book Tracker is a shared household system for tracking physical books and family reading activity. It helps a family quickly identify a book by barcode or search, determine whether the household owns it, see who has read it or wants to read it, view reviews, and track whether a book has been loaned out.

The product consists of:

- A mobile app for fast scanning, quick lookups, and lightweight updates
- A web app for administration, bulk management, imports/exports, and deeper editing
- A shared backend and database used by both clients

## Problem Statement

Households with many books often do not know:

- Whether they already own a specific book
- Whether someone in the family has read it
- Whether someone wants to read it
- Whether a book has been loaned out
- What a family member thought of a book after reading it

This leads to duplicate purchases, poor visibility into the family library, and weak tracking of reading activity and book loans.

## Goals

- Let a user scan a book barcode and get an immediate answer about household ownership and reading status
- Support manual lookup by title, author, or ISBN when barcode scanning is unavailable or fails
- Track per-family-member reading status and reviews
- Track household ownership and current loan state
- Make mobile use fast enough for bookshelf, store, and lending scenarios
- Provide a web interface for administration, imports/exports, and bulk data maintenance
- Support secure sign-in using Google accounts

## Non-Goals For Initial Version

- Public social network features
- Multi-household sharing between unrelated families
- Marketplace or book purchasing integrations
- Advanced recommendation engine
- OCR-based spine recognition
- Deep offline-first sync as a launch requirement

## Users

Primary household members:

- Parent 1
- Parent 2
- Child 1
- Child 2

Potential future users:

- Additional household members
- Temporary guest accounts

## Roles

### Admin

Likely one or two adults in the household.

Permissions:

- Manage family members
- Manage roles
- Import/export library data
- Edit incorrect book metadata
- Merge duplicates
- Manage household settings

### Member

Regular household member.

Permissions:

- Sign in
- View library
- Scan and search books
- Update own reading state
- Add own review or rating
- Mark books as wanted
- View family reading and loan status
- Record or update loans if allowed by policy

## Core User Questions

For any book, the product should answer:

- Do we own it?
- Have I read it?
- Has anyone else in the family read it?
- Is there a review or rating from a family member?
- Does anyone in the family want to read it?
- Is it currently loaned out?
- If it is loaned out, to whom and since when?

## Product Surfaces

### Mobile App

Primary purpose:

- Fast, frequent, low-friction actions

Key capabilities:

- Scan barcode with camera
- Search by title, author, or ISBN
- Show immediate book summary
- Mark as read, reading, want to read, or not read
- Add a short rating or review
- Confirm household ownership
- Record that a book is loaned out or returned
- Use bulk intake mode for repeated scanning
- Browse and filter library

Recommended mobile-first workflows:

- In a bookshop: scan and see if the household already owns or has read the book
- At home: scan a shelf of books and mark them as owned
- During family reading: quickly update read and want-to-read states
- When lending: record borrower and later mark returned

### Web App

Primary purpose:

- Deeper administration and data management

Key capabilities:

- View full library table with richer filtering and sorting
- Import library data from CSV
- Export library data to CSV
- Manage family members and roles
- Correct or enrich metadata
- Merge duplicate books or editions
- Review loans and statuses at scale
- View household-level summaries and reports

Recommended web-first workflows:

- Initial setup and cleanup of the library
- Bulk corrections after imports
- Reviewing duplicates or incomplete metadata
- Managing family access

## Feature Set By Area

### 1. Book Identification

Requirements:

- Barcode scanning on mobile
- Fallback search by title, author, or ISBN
- Ability to choose the correct match if lookup returns multiple editions
- Persist metadata after lookup so it does not need to be fetched repeatedly

MVP source:

- Use Google Books API as the initial external metadata source for search and lookup

Nice later enhancements:

- Cover image enrichment
- Smarter edition matching
- Manual metadata override workflow

### 2. Ownership Tracking

Requirements:

- Track whether the household owns a book
- Track format such as physical, ebook, or audiobook

Design note:

- Ownership should be modelled at household level, even if a specific member purchased the book
- MVP ownership should be tracked at title level rather than copy level

### 3. Reading Tracking

Per family member, the system should support:

- Not started
- Reading
- Read
- Abandoned
- Want to read

Optional metadata:

- Date started
- Date finished
- Rating
- Review

### 4. Reviews And Ratings

Requirements:

- A family member can leave a review for a book
- Reviews should be visible to the family
- Ratings should be lightweight to enter on mobile

MVP decision:

- Reviews are shared within the household by default

### 5. Wish-list / Want-To-Read

Requirements:

- A member can mark a book as wanted to read
- Other family members should be able to see this signal
- The book detail should show which members want to read it

### 6. Loans

Requirements:

- Record whether a book is currently loaned out
- Store borrower name
- Store loan date
- Mark the book as returned

Later enhancement:

- Loan reminders or overdue nudges

Explicitly out of MVP:

- Shelf or location tracking

### 7. Family Accounts And Access Control

Requirements:

- Google Sign-In for all members
- Household membership managed by the application
- Access limited to invited or approved Google accounts
- Role-based permissions for admin vs member

MVP decision:

- Children do not have restricted permissions in the initial release

## Primary User Flows

### Flow A: Scan To Check Status

1. User opens mobile app.
2. User scans barcode.
3. App finds book by ISBN or barcode lookup.
4. App shows a summary:
   - Owned or not owned
   - Read by me or not
   - Read by family members
   - Wanted by family members
   - Loaned out or available
5. User optionally updates status.

### Flow B: Search When Barcode Fails

1. User opens search.
2. User enters title, author, or ISBN.
3. App shows possible matches.
4. User chooses a match.
5. App shows the same summary and quick actions as scan flow.

### Flow C: Bulk Intake On Mobile

1. User enters bulk intake mode.
2. User scans one book after another.
3. App finds or creates entries.
4. User quickly confirms metadata if needed.
5. App marks book as owned.
6. App optionally applies a common shelf/location.
7. App returns immediately to camera for next scan.

### Flow D: Manage Library On Web

1. Admin signs into web app.
2. Admin opens library table.
3. Admin filters, edits, imports, exports, or merges records.
4. Changes become immediately available in mobile app.

## MVP Recommendation

The first version should include:

- Google login
- Household membership
- Barcode scanning on mobile
- Title/author/ISBN search fallback
- Shared library with household ownership
- Per-member reading status
- Per-member want-to-read status
- Ratings and short reviews
- Loan tracking
- Web-based imports/exports
- Family member management

## Deferred Features

Defer until after MVP unless they become critical:

- Push notifications
- Reminders for loans
- Rich recommendation engine
- Advanced analytics dashboards
- OCR spine recognition
- Offline-first conflict resolution
- Multi-household support
- Offline mobile scanning
- Shelf or location tracking

## High-Level Domain Model

Main concepts:

- Family
- User
- FamilyMember
- Book
- BookEdition
- OwnedBook or BookCopy
- ReadingStatus
- Review
- WantToRead
- Loan
- ShelfLocation
- ImportJob
- ScanSession

Important modelling guideline:

- Separate the abstract work from the owned copy when possible
- This keeps edition and loan tracking cleaner over time

## Success Criteria

The product is successful if household members can:

- Check a book in seconds while standing in front of a shelf or store display
- Reliably know whether the household owns the book
- See whether anyone in the family has read or wants the book
- Record reading and loan activity with very little friction
- Manage the shared library without spreadsheet-heavy administration

## Resolved MVP Decisions For Technical Design

- Ownership starts at title level, not copy level
- Children do not have restricted permissions in MVP
- Reviews are shared by default within the household
- Shelf or location tracking is not included in MVP
- Offline mobile scanning is not required for launch
- Google Books API is the initial external metadata source

## Suggested Next Planning Documents

- Technical design
- Delivery roadmap
- Infrastructure plan
- Detailed backlog by feature area
