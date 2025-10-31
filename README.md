# AI_RMS — AI-Assisted Resume Management System

This is a multi-tenant, AI-powered Resume Management System (RMS) built with:
- Next.js (JavaScript only) for frontend
- Express.js + Prisma (MongoDB Atlas) for backend
- Ollama (Gemma3) and Whisper/Vosk for local AI text/voice processing

Monorepo structure:
- /apps/web       → Next.js frontend (Persian RTL, blue-white theme)
- /apps/api       → Express.js backend
- /packages/*     → shared UI, utilities, AI helpers
- /docs           → project documentation & memory
- /scripts        → deployment scripts
"@ | Out-File -Encoding UTF8 README.md
