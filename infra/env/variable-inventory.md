# Variable Inventory

## Purpose

This file is the canonical list of environment variables used by the project. Update it whenever a new variable is introduced.

Only variable names, usage, and descriptions belong in this file. Never place real secret values in this document.

## Variables

| Variable                   | Required In                      | Used By                      | Description                                             | Secret |
|----------------------------|----------------------------------|------------------------------|---------------------------------------------------------|--------|
| `DATABASE_URL`             | development, preview, production | `apps/api`, database tooling | Primary database connection string                      | yes    |
| `DIRECT_DATABASE_URL`      | development, preview, production | database tooling             | Direct database connection string if Prisma requires it | yes    |
| `GOOGLE_WEB_CLIENT_ID`     | development, preview, production | `apps/web`, `apps/api`       | Google OAuth client ID for web sign-in                  | no     |
| `GOOGLE_WEB_CLIENT_SECRET` | development, preview, production | `apps/api`                   | Google OAuth client secret for web sign-in              | yes    |
| `GOOGLE_MOBILE_CLIENT_ID`  | development, preview, production | `apps/mobile`, `apps/api`    | Google OAuth client ID for mobile sign-in               | no     |
| `SESSION_SECRET`           | development, preview, production | `apps/api`                   | Secret used to sign or encrypt application sessions     | yes    |
| `API_BASE_URL`             | development, preview, production | `apps/mobile`, `apps/web`    | Base URL for the backend API                            | no     |
| `WEB_BASE_URL`             | development, preview, production | `apps/api`, `apps/web`       | Public base URL for the web app                         | no     |

## Storage Rules

- Real values belong in local `.env.local` files or hosted environment variable settings
- `.env.example` files may contain placeholder values only
- Downloaded credential files from providers should not be stored in the repository
- If a secret is exposed in git history, treat it as compromised and rotate it

## Future Variables

Add later only if needed:

| Variable            | Required In         | Used By       | Description                           | Secret |
|---------------------|---------------------|---------------|---------------------------------------|--------|
| `SENTRY_DSN_WEB`    | preview, production | `apps/web`    | Sentry DSN for web error reporting    | yes    |
| `SENTRY_DSN_API`    | preview, production | `apps/api`    | Sentry DSN for API error reporting    | yes    |
| `SENTRY_DSN_MOBILE` | preview, production | `apps/mobile` | Sentry DSN for mobile error reporting | yes    |
