<!-- markdownlint-disable -->

# Hardening Report: clowdhaus--terraform-min-max/v1.4.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **clowdhaus--terraform-min-max/v1.4.1** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All `uses:` references in release.yml use mutable tags instead of pinned 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks if the referenced action tags are moved. Failing references: `actions/checkout@v4`, `actions/setup-node@v4`.

Locations:

- `.github/workflows/release.yml:17`
- `.github/workflows/release.yml:22`

### unpinned-uses (severity: high)

All `uses:` references in test.yml use mutable tags instead of pinned 40-character commit SHAs, making the workflow vulnerable to supply-chain attacks. Failing references: `actions/checkout@v4` (×2), `clowdhaus/terraform-composite-actions/directories@v1.11.1`, `hashicorp/setup-terraform@v3` (×2).

Locations:

- `.github/workflows/test.yml:18`
- `.github/workflows/test.yml:23`
- `.github/workflows/test.yml:33`
- `.github/workflows/test.yml:46`
- `.github/workflows/test.yml:58`

### missing-permissions (severity: medium)

The workflow file release.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/release.yml:1`

### missing-permissions (severity: medium)

The workflow file test.yml has no top-level `permissions:` key and no job-level `permissions:` key on any job. Without explicit permissions, the GITHUB_TOKEN is granted default (potentially broad) permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings across two workflow files:

**release.yml:**
- Added `permissions: contents: write` (top-level) — semantic-release needs write access to create tags/releases
- Pinned `actions/checkout@v4` → `@11d5960a326750d5838078e36cf38b85af677262 # v4`
- Pinned `actions/setup-node@v4` → `@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`

**test.yml:**
- Added `permissions: contents: read` (top-level) — minimal permission for checkout and testing
- Pinned `actions/checkout@v4` (×2) → `@11d5960a326750d5838078e36cf38b85af677262 # v4`
- Pinned `clowdhaus/terraform-composite-actions/directories@v1.11.1` → `@8aef6513928e6e56c30662afb34a21bf4bf5d9f4 # v1.11.1`
- Pinned `hashicorp/setup-terraform@v3` (×2) → `@b9cd54a3c349d3f38e8881555d616ced269862dd # v3`

