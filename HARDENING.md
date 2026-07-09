<!-- markdownlint-disable -->

# Hardening Report: clowdhaus--terraform-min-max/v1.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **clowdhaus--terraform-min-max/v1.0.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file uses action references pinned to mutable tags rather than immutable full 40-character commit SHAs. This exposes the workflow to supply-chain attacks if the upstream tag is moved or the repository is compromised. Failing references: `actions/checkout@v2` (lines 15 and 38), `hashicorp/setup-terraform@v1` (line 40). Each should be replaced with a full SHA pin, e.g. `actions/checkout@<40-hex-sha> # v2`.

Locations:

- `.github/workflows/test.yml:15`
- `.github/workflows/test.yml:38`
- `.github/workflows/test.yml:40`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and neither job (`versionExtract` nor `versionEvaluate`) defines its own `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (write access to contents, packages, etc.). A minimal `permissions:` block (e.g. `contents: read`) should be added at the top level or per job.

Locations:

- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed hardened/action/.github/workflows/test.yml: (1) Added top-level `permissions: contents: read` block to address missing-permissions finding. (2) Pinned actions/checkout@v2 to @ee0669bd1cc54295c223e0bb666b733df41de1c5 # v2 (both occurrences on lines 15 and 38). (3) Pinned hashicorp/setup-terraform@v1 to @ed3a0531877aca392eb870f440d9ae7aba83a6bd # v1 (line 40). All SHAs were resolved using lookup_action_sha.

