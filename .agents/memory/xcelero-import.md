---
name: xCelero Next.js Import
description: Key decisions made when importing a standalone Next.js+Bun app into the pnpm monorepo workspace
---

The workspace now has two independent app stacks:
1. pnpm workspace (Express + Drizzle + React Vite)
2. `artifacts/celero/` — standalone Next.js 16 + Bun + Prisma

**Key constraints:**
- Never add `artifacts/celero` to `pnpm-workspace.yaml` — it's a Bun project, not pnpm
- `artifacts/celero/.env` must NOT include `DATABASE_URL` — Replit manages it as a secret pointing to PostgreSQL. The original repo had a SQLite file URL there which breaks the Replit DB connection
- Prisma schema was changed from `sqlite` to `postgresql` provider and pushed with `bun run db:push`
- `next.config.ts` changes: removed `X-Frame-Options: DENY` header (blocks preview iframe) and `frame-ancestors 'none'` from CSP; added `*.replit.dev` to `allowedDevOrigins`
- `verifyAndReplaceArtifactToml` requires the `integratedSkills` field to be preserved — it cannot be removed
- The artifact.toml dev run is `bun run next dev -p <port>` (not pnpm)
- The `bun.lock` in `artifacts/celero/` causes a harmless Next.js workspace root warning about multiple lockfiles

**Why:** Next.js + Bun can't share a pnpm lockfile. Keeping them separate avoids version conflicts and lets each use their own dependency manager.

**How to apply:** When working on the xCelero app, `cd artifacts/celero` and use `bun` commands. When working on the shared workspace, use `pnpm` from the root.
