# Migration Roadmap

## Phase 0 — Foundations

- Initialize workspace tooling:
  - `pnpm-workspace.yaml`
  - Base `package.json` with scripts for lint/test/build.
  - Shared linting/formatting (`packages/config`).
- Establish folder layout:
  - `apps/`
  - `services/`
  - `packages/`
  - `infrastructure/`
  - `docs/`
- Configure CI (GitHub Actions) skeleton.

## Phase 1 — Shared Packages

- `packages/config`: eslint, prettier, tsconfig presets.
- `packages/types`: domain types shared across services/apps.
- `packages/utils`: cross-cutting helpers (logging, feature flags, formatting).
- `packages/db`: Prisma schema split by domain, generator scripts.
- Add unit test harness (`vitest` or `jest`) shared across packages.

## Phase 2 — Frontend Extraction

- Create `apps/web` based on the legacy Next.js app.
- Replace inline API route logic with calls to new services.
- Move UI primitives to `packages/ui`.
- Introduce Storybook or component testing for shared UI (optional but recommended).

## Phase 3 — Backend Services

- `services/search-api`: vector, hybrid, semantic search endpoints with adapters to external providers.
- `services/auth-api`: session management, OAuth integrations, password reset flows.
- `services/payments-api`: Stripe webhooks, subscription logic.
- `services/workspace-api`: project management, collaboration features.
- Shared middleware for observability, error handling, and auth tokens.

## Phase 4 — Infrastructure & Dev Experience

- Docker Compose for local multi-service orchestration.
- Environment variable templates per service.
- GitHub Actions: build/test matrix, lint gate, deploy previews.
- Observability stack (logging, tracing) decisions documented.

## Phase 5 — Parity & Cleanup

- Run regression tests comparing legacy vs modular services.
- Update documentation, diagrams, and onboarding materials.
- Sunset unused legacy artifacts; archive the mono-repo or mark read-only.

## Tracking Progress

- Use repo Issues/Projects to break phases into actionable tasks.
- Maintain changelog summarizing migrations.
- Keep docs updated alongside code to avoid drift.

