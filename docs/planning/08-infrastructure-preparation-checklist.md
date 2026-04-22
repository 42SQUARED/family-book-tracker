# Infrastructure Preparation Checklist

## Purpose

This checklist is the practical sequence for preparing the real infrastructure needed before coding begins.

## Order Of Execution

1. Create the GitHub repository
2. Create the Vercel account/projects
3. Create the Neon database
4. Create the Google Cloud project
5. Enable Google Books API
6. Create Google OAuth clients
7. Fill in `infra/env/setup-record.md`
8. Fill in `infra/env/variable-inventory.md` only if new variables are introduced
9. Confirm all secrets exist in local and hosted environments

## Step 1: GitHub

Do:

- create a new repository
- use `main` as the default branch
- keep it private

Record into setup record:

- repository URL
- owner account or organization

## Step 2: Vercel

Do:

- create or use a Vercel account
- import the repository
- create one project for `apps/web`
- create one project for `apps/api`
- use Hobby tier first

Record into setup record:

- Vercel account or team name
- web project name
- API project name
- preview and production URLs when known

## Step 3: Neon

Do:

- create a Neon project
- create the main database
- keep to the free tier initially

Record into setup record:

- Neon project name
- database name
- connection string references

## Step 4: Google Cloud

Do:

- create a dedicated Google Cloud project for this app
- configure the OAuth consent screen

Record into setup record:

- project name
- project ID
- consent app name

## Step 5: Google Books API

Do:

- enable the Google Books API for the project

Record into setup record:

- mark Google Books API as enabled

## Step 6: Google OAuth Clients

Do:

- create a web OAuth client
- create a mobile OAuth client for the Expo workflow you will use
- add localhost redirect URIs for development
- add hosted redirect URIs later when preview and production URLs are known

Record into setup record:

- web client ID
- mobile client ID
- redirect URIs

## Step 7: Secrets

Create these values:

- `DATABASE_URL`
- `DIRECT_DATABASE_URL` if required
- `GOOGLE_WEB_CLIENT_ID`
- `GOOGLE_WEB_CLIENT_SECRET`
- `GOOGLE_MOBILE_CLIENT_ID`
- `SESSION_SECRET`
- `API_BASE_URL`
- `WEB_BASE_URL`

Store them in:

- local `.env.local` files
- Vercel environment variables

## Completion Check

Infrastructure preparation is complete when:

- all external accounts exist
- all required client IDs and secrets exist
- the setup record is filled in
- no critical value needed by an implementation agent is still unknown
