# Project Summary (Current)

## Status
- Milestone: **M0 — Repo & CI/CD setup**
- State: **Completed (proposal)**, waiting for confirmation to move to M1.
- Scope covered: repository structure, git setup, CI/CD design plan, environment variables, risk register.

## Done
- Initialized monorepo structure (`/apps/web`, `/apps/api`, `/packages/*`, `/docs`, `/scripts`).
- Added README and .gitignore.
- Set up Git repository and pushed to GitHub.
- Prepared documentation skeleton files for memory tracking.

## In-Progress
- Preparing initial contents for `/docs` files (this milestone).

## Next
- **M1: System Admin Console** (companies, admin users, impersonation, audit).

## Risks
1. Data sensitivity → enforce TLS, audit logs.
2. Local AI resource load → rate limits and queued requests (M5–M6).
3. Multi-tenant complexity → strict `companyId` scoping (M2).

## Links
- (TBD) Architecture overview
- (TBD) CI/CD runbook
- (TBD) Deployment guide

## Gate Criteria for Next Milestone
- Repository structure verified.
- CI/CD and env variable design approved.
- Progress tracker initialized.
