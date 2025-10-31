# SUMMARY_CURRENT — Milestone M1 (System Admin Console)

**Scope (M1):** Companies (CRUD), Admin users (create/reset/roles), secure impersonation with audit logging.  
**Tech (per ADR-0001):** Monorepo; Next.js + Express; Prisma on **MongoDB Atlas**.

---

## M1.1 — Minimal Schema & Multi-Tenant Logic (Design Only)

**Status:** Delivered (awaiting confirmation)  
**Artifacts:**
- `/docs/SCHEMA.prisma` — proposal-only draft (MongoDB): `Company`, `User`, `UserRole`, indexes & uniques.
- `/docs/DECISIONS.md` — `ADR-0007` added with context, decision, rationale, security notes, alternatives, future work.

**Key Decisions**
- **Shared DB / shared schema** multi-tenant model.
- **Global unique** `User.email`.
- `Company.slug` unique (URL routing), `Company.domain?` unique (future SSO/domain mapping).
- **Platform admins:** `isSystemAdmin=true`, `companyId=null`.
- **Tenant users:** `companyId` **MUST** be set.

**Indexes & Constraints**
- `Company`: `slug` (unique), `domain?` (unique), `@@index([name])`
- `User`: `email` (unique), `@@index([companyId])`, `@@index([role])`, `@@index([isSystemAdmin])`

**Out of Scope (Planned)**
- `AuditLog` model (M1.3)
- Soft-delete flags, data retention policies (later)
- `UserCompany` (many-to-many memberships) (later)

**Risks & Mitigations**
- Mongo relations not DB-enforced → enforce in app layer (repositories/services).
- Global email uniqueness may limit cross-tenant reuse → introduce `UserCompany` later if needed.

**Acceptance Checklist (M1.1)**
- [x] Schema proposal (Mongo) written
- [x] Multi-tenant rationale documented (ADR-0007)
- [x] Indexes/uniques defined
- [x] No runtime code/migrations added
- [x] Next steps outlined

---

## Progress (M1 Internal Breakdown)

> Total M1 Weight per ADR-0006: **9%** of overall project.  
> Suggested split inside M1 (for tracking):
- **M1.1 — 25%** (design, schema & tenancy) → _Delivered, awaiting confirmation_
- **M1.2 — 35%** (API shapes)
- **M1.3 — 25%** (AuditLog model + impersonation audit wiring)
- **M1.4 — 15%** (Security notes + PR/commit plan + wrap-up)

**Current M1 completion:** ~**25%** (pending confirmation)

---

## Next Steps
- Upon confirmation of M1.1: proceed to **M1.2**  
  - Define API shapes for `/system/companies`, `/system/admins`, `/system/impersonation` (design only, JSON contracts, error model, headers).

