# Infrastructure Setup Guide

## Purpose

This document lists the infrastructure, external accounts, credentials, and environment information that must exist before implementation begins. It is written to make agent-led execution practical and low-ambiguity.

## Operating Constraints

- this is a private family app
- cost should stay as close to `$0` as possible for now
- production-grade paid services should be avoided unless free-tier limits become a real problem
- if the app is later monetized, the infrastructure and cost model must be revisited

## Required External Services

### 1. Source Control

Purpose:

- store the monorepo
- trigger CI
- act as the source of truth for AI-agent work

Recommendation:

- GitHub

Required setup:

- create repository
- choose owner account or organization
- enable branch protection later when implementation starts
- decide default branch name

Required recorded outputs:

- repository host
- repository URL
- owner account or organization
- default branch name

### 2. Hosting

Purpose:

- deploy `apps/web`
- deploy `apps/api`

Recommendation:

- Vercel

Required setup:

- create Vercel account or team
- connect repository
- create separate projects for `apps/web` and `apps/api`
- define environment names: `development`, `preview`, `production`

Required recorded outputs:

- Vercel team or account name
- web project name
- API project name
- linked repository

### 3. Database

Purpose:

- host PostgreSQL

Recommendation:

- Neon on free tier first

Required setup:

- create Neon account
- create project for the app
- create main database
- record connection strings for local and hosted apps

Required recorded outputs:

- Neon project name
- database name
- connection string names

### 4. Google Cloud

Purpose:

- Google Sign-In
- Google Books API access

Recommendation:

- one dedicated Google Cloud project for this app

Required setup:

- create Google Cloud project
- configure OAuth consent screen
- enable Google Books API
- create OAuth client for web
- create OAuth client for Expo/mobile workflow as needed by the chosen implementation

Required recorded outputs:

- Google Cloud project ID
- OAuth consent app name
- web client ID
- mobile client ID
- any redirect URIs in use

## Credentials And Secrets Inventory

These values should be created and documented before implementation:

- `DATABASE_URL`
- `DIRECT_DATABASE_URL` if required by Prisma workflow
- `GOOGLE_WEB_CLIENT_ID`
- `GOOGLE_WEB_CLIENT_SECRET`
- `GOOGLE_MOBILE_CLIENT_ID`
- `SESSION_SECRET`
- `API_BASE_URL`
- `WEB_BASE_URL`

Potential later additions:

- `SENTRY_DSN_WEB`
- `SENTRY_DSN_API`
- `SENTRY_DSN_MOBILE`

## Secret Storage Strategy

Use two layers:

- local developer secrets in uncommitted `.env.local` files
- hosted secrets in Vercel environment variables

Rules:

- never commit live secrets to git
- commit `.env.example` files with all required variable names and placeholder values
- keep one canonical document listing every variable, where it is used, and which environments require it

## What Must Not Be Committed To Git

The repository must never contain:

- real `.env` or `.env.local` files
- database connection strings
- OAuth client secrets
- session secrets
- service-account JSON files
- downloaded credential files such as `client_secret.json`
- production API keys or DSNs
- local database files created for development

What is safe to commit:

- `.env.example` files with placeholder values only
- client IDs that are intended to be public identifiers
- documentation that lists variable names without real values
- setup instructions and redirect URI lists

Enforcement:

- keep a root `.gitignore` that excludes secret-bearing local files
- prefer copying provider credentials into environment variables rather than storing downloaded files in the repo
- if a secret is ever committed accidentally, rotate it instead of only deleting the file locally

## Environment Model

Use three named environments from the start:

### Development

Purpose:

- local development on laptops

Typical characteristics:

- local app processes
- shared or local development database
- non-production OAuth redirect URIs

### Preview

Purpose:

- temporary hosted builds for testing

Typical characteristics:

- Vercel preview deployments
- separate environment variables from production

### Production

Purpose:

- live household usage

Typical characteristics:

- production Vercel deployments
- production database connection string
- production OAuth redirect URIs

## Repository-Level Prerequisites

Before implementation begins, the repo should contain:

- `README.md`
- `docs/planning/`
- `.gitignore`
- `.editorconfig`
- top-level package manager files
- workspace configuration
- `.env.example` files for each app

Recommended future files:

- `infra/env/variable-inventory.md`
- `infra/scripts/bootstrap-dev.sh`
- `.github/workflows/ci.yml`

The repo should not contain:

- live secret values
- provider credential downloads
- private key files
- local-only environment files

## AI-Agent Readiness Checklist

An AI agent should not start implementation until the following are provided:

- repository exists and is writable
- package manager choice is fixed
- hosting provider account exists
- database provider account exists
- Google Cloud project exists
- Google OAuth clients exist
- Google Books API is enabled
- required secrets list is complete
- environment names are fixed

## Human Decisions Still Required

These are not code tasks and should be explicitly decided by the project owner:

- GitHub owner account or organization
- Vercel personal account or team
- Neon project name
- Google Cloud project name
- production app URLs once reserved

## Recommended Completion Artifact

Before handing work to implementation agents, create one machine-readable or plain-text setup record containing:

- repository URL
- branch strategy
- package manager
- app URLs by environment
- database project details
- OAuth client IDs
- secret variable names
- where each secret is stored
- any manual setup steps still pending

Suggested file:

- `infra/env/setup-record.md`

## Minimal Near-Zero-Cost Setup

For the current private-family phase, the recommended starting posture is:

- GitHub repository on an existing personal account
- Vercel Hobby
- Neon Free
- Google Cloud project with only required APIs enabled
- no paid monitoring initially

This should keep recurring infrastructure cost near `$0`, subject to provider limits.
