# lockfile-sync-poc

Minimal npm workspaces monorepo intended to reproduce a Dependabot security update failure on transitive `lodash`.

## Structure

- `apps/cms` is a workspace app with public Sanity packages.
- `packages/shared` is a local workspace package.
- `packages/eslint-config` is a small internal config package.
- The repository uses Turborepo with a single root `package-lock.json`.

## Why this repo exists

In the original monorepo, Dependabot security updates for `lodash` reported:

- `The lockfile might be out of sync?`
- `latest-resolvable-version: 4.17.23`
- `lowest-non-vulnerable-version: 4.18.0` or `4.18.1`

even though the transitive `lodash` ranges in `package-lock.json` allowed a higher version such as `^4.17.21`.

This repo removes:

- private npm registries
- private packages
- company-specific names

while keeping the same core setup:

- npm workspaces
- Turborepo
- root lockfile
- public packages that depend on `lodash`

## Repro outline

1. Keep a committed `package-lock.json` that resolves transitive `lodash` to `4.17.23`.
2. Enable Dependabot security updates for npm.
3. Wait for the `CVE-2026-4800` alert on `lodash`.
4. Observe whether Dependabot reports that the lockfile might be out of sync instead of updating `lodash` to a patched version.

## Local commands

```bash
npm install --package-lock-only --ignore-scripts
npm run build
npm run lint
npm run typecheck
```
