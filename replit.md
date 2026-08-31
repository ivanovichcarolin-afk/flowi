# Flowi

Organizador personal con IA para convertir pendientes y pensamientos en un día más claro, enfocado y productivo.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server
- `pnpm --filter @workspace/flowi run dev` — run the Flowi frontend
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- Supabase schema: run `supabase/schema.sql` once in the Supabase SQL editor
- Required env: `SUPABASE_URL`, `SUPABASE_ANON_KEY`, `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, `ANTHROPIC_API_KEY`

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: Supabase Postgres + REST API with row-level security
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/flowi/src/pages/` — dashboard and Supabase email/password auth screens
- `artifacts/api-server/src/routes/flowi.ts` — authenticated task, AI, Pomodoro, dashboard, and stats endpoints
- `artifacts/api-server/src/lib/supabase.ts` — token-aware Supabase REST helper
- `supabase/schema.sql` — usuarios, tareas, sesiones_pomodoro, RLS policies, and signup trigger
- `lib/api-spec/openapi.yaml` — source of truth for the API contract
- `artifacts/flowi/src/index.css` — Flowi visual theme

## Architecture decisions

- Supabase Auth is handled in the browser; the Express API validates the bearer token against Supabase before accessing user-scoped rows.
- The Supabase publishable key is safe for the browser, while the Anthropic key is server-only and read from `ANTHROPIC_API_KEY`.
- AI usage resets on the first request after the calendar date changes and is limited to five calls for free users.

## Product

Flowi includes email/password auth, task CRUD with normal/urgent priority, AI day planning, brain-dump organization, a 25/5 Pomodoro timer, weekly stats, streak tracking, focus hours, and a five-query daily free tier with Pro bypass.

## User preferences

- Keep the user-facing experience in Spanish.
- Primary brand color is `#378ADD`; the Flowi wordmark uses a blue `i`.

## Gotchas

- Apply `supabase/schema.sql` in the configured Supabase project before testing authenticated API calls.
- Never expose `ANTHROPIC_API_KEY` to the browser or commit it to the repository.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
