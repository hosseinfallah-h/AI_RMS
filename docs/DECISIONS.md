# Architectural Decisions Record (ADR)

## ADR-0001 Monorepo Structure
- Single repo with `/apps/web`, `/apps/api`, `/packages/*`, `/docs`, `/scripts`.
- Next.js (frontend, JavaScript only) + Express (backend) + Prisma (MongoDB Atlas).
- Persian RTL UI with Tailwind + postcss-rtl.

## ADR-0002 Local AI Stack
- Ollama with Gemma3 (1B or 4B) for structured extraction.
- Whisper.cpp or Vosk for STT (speech-to-text).
- ffmpeg for audio preprocessing.

## ADR-0003 CI/CD Plan
- GitHub Actions for CI (lint, build, test).
- Deploy via SSH + PM2 (no Docker).
- Manual approval required on `main` merges.

## ADR-0004 Security & Validation
- JWT (httpOnly) + CSRF.
- helmet, express-validator, ajv.
- Rate limiting per tenant (AI/STT/export).

## ADR-0005 Export & QR
- Excel exports via `exceljs` (UTF-8, RTL).
- Include `tags[]` column in exported data.
- QR codes via `qrcode` library.

## ADR-0006 Progress Tracker Weights
| Milestone | Description | Weight % |
|------------|--------------|----------:|
| M0 | Repo & CI/CD setup | 6 |
| M1 | System Admin console | 9 |
| M2 | Auth & Roles, tenant scoping | 7 |
| M3 | Job Ads + Field Schema + Tags | 12 |
| M4 | Applicant Form Mode | 8 |
| M5 | AI Text Mode | 12 |
| M6 | AI Voice Mode | 10 |
| M7 | Resume Table + Filters | 12 |
| M8 | Excel Export (with tags) | 10 |
| M9 | Landing + Docs + Accessibility | 6 |
| M10 | Online Interview placeholder | 8 |
| **Total** | | **100** |

---

## ADR-0007 Minimal Schema & Multi-Tenant Model (M1.1)

### Context
Milestone **M1** introduces the System Admin Console which must manage multiple companies (tenants), platform-level admins, and later support secure impersonation with full auditability. Per **ADR-0001**, the stack uses **Prisma + MongoDB Atlas**. We need a minimal, evolvable schema and a clear tenant isolation strategy.

### Decision
Adopt a **single database / shared schema** multi-tenant model where entities carry a `companyId` for row-level scoping. Introduce the following minimal models (see `/docs/SCHEMA.prisma`):
- `Company { id, name, slug(unique), domain(unique), createdAt, updatedAt }`
- `User { id, email(unique), displayName, role(UserRole), isSystemAdmin, companyId?, createdAt, updatedAt }`
- `UserRole = OWNER | ADMIN | MANAGER | MEMBER`

Rules:
- **Tenant users** MUST have `companyId` set.
- **System admins** have `isSystemAdmin = true` and SHOULD have `companyId = null` (operate outside single-tenant scope).

### Rationale
- **Operational simplicity (M1)**: One DB, fewer migrations, quick iteration.
- **Security clarity**: A single global identity space (unique `email`) avoids duplicate accounts across tenants and simplifies impersonation/audit later.
- **Evolvability**: Can introduce `UserCompany` (many-to-many) and `AuditLog` later without breaking current design.

### Implications
- **Indexing/Uniqueness**:
  - `User.email` globally unique.
  - `Company.slug` unique (URL-safe routing).
  - `Company.domain` optional unique (future SSO or domain mapping).
  - Helpful indexes on `User.companyId`, `User.role`, `User.isSystemAdmin`, `Company.name`.
- **MongoDB/Prisma**: Relations are application-level (not enforced by DB); Prisma models define references and we rely on application logic for integrity.
- **Auth & RBAC (to be detailed in M2)**: Authorization checks must gate `/system/*` endpoints to system admins; tenant routes must filter by `companyId`.

### Security Notes (M1 scope)
- **Minimal PII** in `User` at this stage.
- **Row-level scoping** by `companyId` must be consistently applied in queries and repository layers.
- **Impersonation** is planned; all actions during an impersonation session must be attributable (via forthcoming `AuditLog`).

### Alternatives Considered
1) **Schema-per-tenant**: Higher isolation; heavier ops, migrations, CI/CD overhead—deferred.
2) **Database-per-tenant**: Strong isolation; significant complexity for routing and scale—deferred.
3) **Company-scoped unique emails**: Simpler migrations for mergers but complicates global identity/impersonation—rejected for M1.

### Future Work (Planned)
- **M1.3**: Add `AuditLog` model and integrate impersonation lifecycle (`start/stop`) with audit entries.
- **M2**: RBAC refinements, CSRF strategy, auth header contracts (including impersonation context).
- **Later**: Optional `UserCompany` for multi-company memberships; soft-delete flags; data retention policies.

### Rollback Plan
If shared-schema scoping proves insufficient, migrate to a schema-per-tenant model with automated migration tooling; retain global admin database for `/system/*` operations.

### References
- `/docs/SCHEMA.prisma` (M1.1.a proposal)
- ADR-0001 (Monorepo & Technology stack)
