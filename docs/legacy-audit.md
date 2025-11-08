# Legacy Monolith Audit

Legacy repository: `../Ecosyz-search`

## High-Level Domains

- **Next.js app (`app/`)**: combines frontend routes and serverless API handlers.
- **Shared libraries (`app/lib`, `src/lib`, `src/types`)**: utilities for search, analytics, payments, workspace management.
- **API routes (`app/api/*`)**: mixed responsibilities (auth, search, analytics, payments, docs, deployments).
- **Prisma schema (`prisma/`)**: single schema powering all features; migrations tightly coupled.
- **Scripts & tests (`tests`, `scripts`, shell helpers)**: integration tests for search, auth, and vector services.
- **Docs (`docs/`)**: extensive guides covering architecture, deployments, testing, roadmaps.
- **Generated projects (`generated-projects/`)**: export artifacts from AI-assisted scaffolding.

## Identified Modules for Extraction

| Domain | Current Location | Proposed Destination |
| --- | --- | --- |
| Web frontend | `app/**` | `apps/web` |
| Admin/dashboard (if needed) | `app/dashboard`, `app/workspaces` | `apps/admin` (future) |
| Search APIs (semantic, hybrid, vector) | `app/api/search`, `app/api/hybrid-search`, `app/api/huggingface-search`, `lib/search/*` | `services/search-api` |
| Analytics & telemetry | `app/api/analytics`, `lib/analytics` | `services/analytics-api` |
| Auth flows | `app/api/auth`, `app/auth`, `middleware.ts` | `services/auth-api` |
| Payments & billing | `app/api/payments`, `lib/payments` | `services/payments-api` |
| Workspace management | `app/api/workspaces`, `app/workspaces`, `lib/workspaces` | `services/workspace-api` |
| Shared UI components | `app/components/**` | `packages/ui` |
| Shared React hooks | `app/hooks`, `hooks/use-toast.ts` | `packages/hooks` |
| Shared domain logic | `app/lib`, `src/lib`, `app/types`, `src/types` | `packages/core` |
| Configuration & environment | `next.config.ts`, `supabase/`, `env.*`, `docker-compose.*` | `packages/config`, `infrastructure/` |
| Prisma schema/client | `prisma/` | `packages/db` with generated clients per service |

## Observations & Risks

- **Tight coupling**: API routes import UI-layer utilities; refactor to share only type-safe contracts.
- **Stateful middleware**: Auth middleware mixes cookies and session logic; must centralize in `services/auth-api`.
- **Search dependencies**: Hybrid search uses external providers (HuggingFace, OpenAI). Capture environment variables and rate-limiting rules before extraction.
- **Generated artifacts**: Ensure `generated-projects/` remains optional; likely move to `examples/`.
- **Testing gaps**: Few unit tests; mostly integration scripts. Need to introduce service-level tests during rebuild.

## Next Actions

1. Finalize target directory layout (see `docs/migration-roadmap.md`).
2. Decide on workspace tool (`pnpm` vs `npm` vs `yarn`). Legacy repo uses `pnpm-lock.yaml`.
3. Extract shared configuration first to unblock frontend/backend migration.

