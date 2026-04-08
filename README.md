# lockfile-sync-poc

Minimal npm workspaces monorepo intended to reproduce a Dependabot security update failure on transitive `js-yaml`.

## Structure

- `apps/cms` is a workspace app with public Sanity packages.
- `packages/shared` is a local workspace package.
- `packages/eslint-config` is a small internal config package.
- The repository uses Turborepo with a single root `package-lock.json`.

## Why this repo exists

Dependabot security updates in this reduced public repro report:

- `The lockfile might be out of sync?`
- `latest-resolvable-version: 3.13.1`
- `earliest fixed version: 3.14.2`

for the `js-yaml` security advisory `CVE-2025-64718`, even though a patched version exists.

This repo removes:

- private npm registries
- private packages
- company-specific names

while keeping the same core setup:

- npm workspaces
- Turborepo
- root lockfile
- public packages with transitive vulnerable dependencies

## Repro outline

1. Keep a committed `package-lock.json` with the vulnerable transitive dependency.
2. Enable Dependabot security updates for npm.
3. Wait for the `CVE-2025-64718` alert on `js-yaml`.
4. Observe whether Dependabot reports that the lockfile might be out of sync instead of updating `js-yaml` to the patched version `3.14.2`.

## Local commands

```bash
npm install --package-lock-only --ignore-scripts
npm run build
npm run lint
npm run typecheck
```
