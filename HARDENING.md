<!-- markdownlint-disable -->

# Hardening Report: clowdhaus--terraform-min-max/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **clowdhaus--terraform-min-max/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag refs instead of pinned 40-character SHA commit hashes, making the workflows vulnerable to supply-chain attacks if the referenced action tags are moved or compromised.

In `.github/workflows/release.yml`:
- `uses: actions/checkout@v5` (line 16)
- `uses: actions/setup-node@v5` (line 22)

In `.github/workflows/test.yml`:
- `uses: actions/checkout@v5` (line 18, line 30)
- `uses: clowdhaus/terraform-composite-actions/directories@v1.13.0` (line 23)
- `uses: hashicorp/setup-terraform@v3` (line 38, line 50)

Locations:

- `.github/workflows/release.yml:16`
- `.github/workflows/release.yml:22`
- `.github/workflows/test.yml:18`
- `.github/workflows/test.yml:23`
- `.github/workflows/test.yml:30`
- `.github/workflows/test.yml:38`
- `.github/workflows/test.yml:50`

### missing-permissions (severity: medium)

Neither `.github/workflows/release.yml` nor `.github/workflows/test.yml` defines a top-level `permissions:` key, and no individual jobs within these files define job-level `permissions:` keys. Without explicit permissions, workflows run with the default (potentially broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/release.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 7 unpinned action references by pinning to full 40-character SHA hashes with original tags preserved as comments. Added top-level `permissions: {}` to both workflow files to deny all permissions by default, with minimal job-level permissions: release job gets contents/issues/pull-requests write access (for semantic-release), and test jobs get contents: read (for checkout).

