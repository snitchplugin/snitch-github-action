# Snitch for GitHub (internal)

License-gated GitHub Action that scans pull requests for security issues. Runs on the customer's own runner; customer source code never enters Snitch infrastructure.

This README is internal-only. During the private beta the customer-facing page lives at `packages/app/src/routes/plugin/github.beta.tsx` (`noindex`, unlinked from the main nav). Post-beta plan: rename to `plugin/github.tsx`, add to nav, publish the Action to GitHub Actions Marketplace. See `BETA.md` for the beta plan and exit criteria.

## Architecture

See `/Users/ianmuench/.claude/plans/my-friend-said-it-dynamic-brook.md`.

## Build

```sh
npm install
npm run build   # bundles to dist/index.js via @vercel/ncc
npm test
```

## Local dry-run

```sh
act pull_request --container-architecture linux/amd64
```

## Inputs

See `action.yml`. At minimum, customers must supply:
- `snitch-license-key`
- One AI provider key (openrouter, anthropic, openai, google, or copilot)

## Per-repo config

Customers can drop a `.snitch.yml` at their repo root to override defaults. Schema documented at `skills/snitch/references/snitch-yml-schema.md`.

## Salvage notes

This package replaces the dead `packages/snitch-action/`. Salvaged code:
- `src/github.ts` ported from `packages/snitch-action/src/github.ts`
- `src/util/paths.ts` ported `isPathWithinRepo` from `packages/snitch-action/src/index.ts:45-48`

Everything else is new.
