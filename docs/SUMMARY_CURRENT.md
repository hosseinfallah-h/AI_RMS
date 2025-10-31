# Project Summary (Current)

## Status
- Milestone: **M0 — Repo & CI/CD setup**
- State: **Ready to close M0** (awaiting confirmation to start M1).
- Scope covered: repository structure, git setup, CI/CD design plan, environment variables, risk register, progress tracker, commit/PR plan.

## Done
- Initialized monorepo structure (/apps/web, /apps/api, /packages/*, /docs, /scripts).
- Added README and .gitignore.
- Set up Git and pushed to GitHub.
- Created memory docs and populated initial contents.
- Added Progress Tracker and Commit/PR plan.

## In-Progress
- None (M0 work is complete pending approval).

## Next
- **M1: System Admin Console** (companies, admin users, impersonation, audit).

## Risks (Top)
1. Data sensitivity → enforce TLS, audit logs, least-privilege.
2. Local AI resource load → rate limits and simple queue (M5–M6).
3. Multi-tenant complexity → strict companyId scoping and indexes (M2).

## Links
- (TBD) Architecture overview
- (TBD) CI/CD runbook
- (TBD) Deployment guide

---

## Progress Tracker (Weights = 100%)
| Milestone | Description | Weight % |
|---|---|---:|
| M0 | Repo & CI/CD setup | 6 |
| M1 | System Admin console (companies, admins, impersonation, audit) | 9 |
| M2 | Auth & Roles; tenant scoping middleware | 7 |
| M3 | Job Ads CRUD + Field Schema builder + Tags per ad + QR | 12 |
| M4 | Applicant Form Mode + attachments | 8 |
| M5 | AI Text Mode extraction pipeline | 12 |
| M6 | AI Voice Mode (record → STT → extraction) | 10 |
| M7 | Admin Resume Table (filters, tags, cursor pagination, bulk) | 12 |
| M8 | Excel Export (Select-All across pages, RTL, tags included) | 10 |
| M9 | Landing page, accessibility, docs | 6 |
| M10 | Online Interview placeholder (AI-only spec) | 8 |
| **Total** |  | **100** |

> Weights represent complexity/time/bug risk, not elapsed time.

---

## Commit & PR Plan (Conventional)
- Branching: main (stable), develop, eature/*, hotfix/*.
- Conventional Commit types: eat, ix, docs, chore, 
efactor, perf, 	est, uild, ci.
- PR rules: clear title, rationale, security/RTL/accessibility checklist, link to relevant docs sections.

### Example PRs for M0 (completed)
1. chore(repo): initialize monorepo structure
2. docs(memory): add initial project memory files
3. docs(plan): describe CI/CD & env inventory
4. docs(tracker): add progress tracker & commit/PR plan

---

## Gate Criteria to Move to M1
- Repository structure verified ✅
- CI/CD and env design accepted ✅
- Progress tracker and commit/PR plan added ✅
- Risks documented ✅

Once approved, proceed to **M1** in a new chat session per project rules.
