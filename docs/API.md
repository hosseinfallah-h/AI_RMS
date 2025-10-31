# API Surface — System & Tenant APIs (M1 Scope)

## Overview
This document defines the logical API surface for the **AI_RMS** backend.
It focuses on **M1 System Admin Console**, covering company and admin user management,
as well as impersonation and audit logging (planned later in M1).

All routes are RESTful JSON endpoints served under `/api/*`.

---

## 1. Namespaces Overview

| Namespace | Description | Access |
|------------|--------------|--------|
| `/auth/*` | Authentication, refresh, logout | All users |
| `/system/*` | Platform-level operations (companies, admin users, impersonation, audit logs) | System Admins only |
| `/ads/*` | Job advertisements, field schema, tags | Company Admins |
| `/resumes/*` | Resume data, filtering, export | Company Admins |
| `/ai/*` | AI-based STT and text extraction | Company Admins |
| `/uploads/*` | File uploads | Authenticated |
| `/qr/*` | QR code generation | Public |
| `/healthz` | Health check endpoint | Public |

---

## 2. M1 Focused Endpoints

### `/system/companies`

| Method | Path | Description |
|--------|------|--------------|
| `GET` | `/system/companies` | List all companies (paginated, filterable) |
| `POST` | `/system/companies` | Create a new company |
| `GET` | `/system/companies/:id` | Get a single company by ID |
| `PATCH` | `/system/companies/:id` | Update company name/domain/slug |
| `DELETE` | `/system/companies/:id` | Delete company (soft-delete planned in later phase) |

#### Example — Create Company
**Request**
```json
{
  "name": "Tech Vision BV",
  "slug": "techvision",
  "domain": "techvision.nl"
}
