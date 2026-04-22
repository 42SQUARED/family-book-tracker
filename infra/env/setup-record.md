# Setup Record

## Purpose

Fill in this file before assigning implementation work to AI agents. It should contain the exact infrastructure facts agents need so they do not guess.

## Recommended Defaults For This Project

- Repository host: `GitHub`
- Default branch: `main`
- Package manager: `pnpm`
- Hosting provider: `Vercel`
- Hosting tier target: `Hobby` first
- Database provider: `Neon`
- Database tier target: `Free` first
- Auth provider: `Google Sign-In`
- Metadata provider: `Google Books API`
- Environments: `development`, `preview`, `production`

## Repository

- Repository host: `GitHub`
- Repository URL: `git@github.com:42SQUARED/family-book-tracker.git`
- Owner account or organization: `42SQUARED`
- Default branch: `main`
- Package manager: `pnpm`

## Environments

### Development

- Web URL: `http://localhost:3000`
- API URL: `http://localhost:4000`
- Database target:

### Preview

- Web URL:
- API URL:
- Database target:

### Production

- Web URL:
- API URL:
- Database target:

## Hosting

- Hosting provider: `Vercel`
- Account or team name:
- Web project name:
- API project name:
- Planned tier: `Hobby`

## Database

- Database provider: `Neon`
- Project name:
- Database name:
- Planned tier: `Free`
- Notes:

## Google Cloud

- Project name:
- Project ID:
- OAuth consent app name:
- Google Books API enabled: `pending`

## OAuth Clients

### Web

- Client ID:
- Redirect URIs:

### Mobile

- Client ID:
- Redirect URIs:

## Secret Storage

- Local secret storage approach: `.env.local` files, never committed
- Hosted secret storage approach: Vercel project environment variables
- Person responsible for secret management:

Rules:

- Do not place real secret values in this file
- Record secret names and storage locations only
- Keep downloaded provider credential files outside the repository

## Pending Manual Steps

- Create GitHub repository
- Create Vercel projects for `apps/web` and `apps/api`
- Create Neon project and database
- Create Google Cloud project
- Enable Google Books API
- Create Google OAuth clients for web and mobile
- Populate environment variables in local and hosted environments

## Notes For Agents

- Do not guess missing URLs, client IDs, or secret names.
- Use `pnpm` workspaces unless the project owner explicitly changes package manager choice.
