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
