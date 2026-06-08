# xCelero Labs

A venture studio and infrastructure platform for building critical technology across emerging markets — imported from GitHub (Questy708/celero1).

## Run & Operate

- `bun run next dev -p 19962` (in `artifacts/celero/`) — run the xCelero Next.js app
- `pnpm --filter @workspace/api-server run dev` — run the shared Express API server (port 8080)
- `pnpm run typecheck` — full typecheck across all pnpm workspace packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas
- `pnpm --filter @workspace/db run push` — push Drizzle DB schema (shared Express app)
- `cd artifacts/celero && bun run db:push` — push Prisma schema (xCelero app, uses Replit PostgreSQL)
- Required env: `DATABASE_URL` — Replit-managed PostgreSQL (do NOT set in artifacts/celero/.env)

## Stack

### xCelero Labs app (`artifacts/celero/`)
- Next.js 16 + Bun runtime
- Prisma ORM → Replit PostgreSQL (`DATABASE_URL`)
- Tailwind CSS v4 + shadcn/ui
- Hash-based SPA routing (no Next.js router beyond root page)
- next-auth, framer-motion, recharts, zustand

### Shared pnpm workspace
- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/celero/` — xCelero Labs Next.js app (standalone, uses Bun)
- `artifacts/celero/src/artemis/` — all page components and data
- `artifacts/celero/src/artemis/router.tsx` — hash-based SPA router
- `artifacts/celero/prisma/schema.prisma` — DB schema (Prisma, PostgreSQL in production)
- `artifacts/celero/.env` — local env overrides (DATABASE_URL must NOT be set here)
- `artifacts/api-server/` — shared Express API server
- `lib/api-spec/openapi.yaml` — OpenAPI contract for shared Express API
- `lib/db/src/schema/` — Drizzle schema for shared Express API

## Architecture decisions

- xCelero is a standalone Next.js app managed with Bun, coexisting with the pnpm workspace
- The `artifacts/celero/.env` file must NOT include `DATABASE_URL` — Replit manages it as a secret pointing to PostgreSQL
- The original SQLite dev setup was replaced with Replit's managed PostgreSQL via `bun run db:push`
- `X-Frame-Options: DENY` was removed from `next.config.ts` to allow preview embedding
- `allowedDevOrigins` updated with `*.replit.dev` for the Replit proxy

## Product

xCelero Labs is a venture studio platform with: company home/manifesto/approach pages, program discovery (Quest Fellowship, xCelerator, Venture Studio, Lab Residency), portfolio ventures, investment routes (Africa/emerging markets), insights/blog, capital raise ($500+ investment tiers), careers, team, admin dashboard, and a Town Square community forum.

## Gotchas

- Run `bun install` (not pnpm) inside `artifacts/celero/` for the Next.js app
- Do NOT add `artifacts/celero` to `pnpm-workspace.yaml` — it's a standalone Bun project
- The `bun.lock` file in `artifacts/celero/` causes a Next.js workspace root warning — harmless
- Prisma schema uses `postgresql` provider (not sqlite) — was updated from the original repo

## Pointers

- See the `pnpm-workspace` skill for shared workspace structure and TypeScript setup
