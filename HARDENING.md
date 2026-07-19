<!-- markdownlint-disable -->

# Hardening Report: Cyb3r-Jak3--html5validator-action/v7.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Cyb3r-Jak3--html5validator-action/v7.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference actions using mutable tags instead of pinned full-length SHA commit digests, making the workflow vulnerable to supply-chain attacks if the tag is moved.

In .github/workflows/action-test.yml:
- `uses: Cyb3r-Jak3/html5validator-action@master` (mutable branch)
- `uses: actions/checkout@v2` (mutable tag)
- `uses: actions/upload-artifact@v2` (mutable tag)

In .github/workflows/integration.yml:
- `uses: actions/checkout@v2` (mutable tag)
- `uses: Cyb3r-Jak3/html5validator-action@master` (mutable branch)

All of these should be pinned to a full 40-character hex commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/action-test.yml:19`
- `.github/workflows/action-test.yml:24`
- `.github/workflows/action-test.yml:67`
- `.github/workflows/integration.yml:13`
- `.github/workflows/integration.yml:15`

### missing-permissions (severity: medium)

Neither .github/workflows/action-test.yml nor .github/workflows/integration.yml defines a top-level `permissions:` key, and no individual job within either file defines a `permissions:` block. Without explicit permissions, the GITHUB_TOKEN is granted its default (potentially broad) permissions, which violates the principle of least privilege.

Locations:

- `.github/workflows/action-test.yml:1`
- `.github/workflows/integration.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

**action-test.yml**:
- Added `permissions: {}` at the top level
- Pinned `Cyb3r-Jak3/html5validator-action@master` → `@443b108eb8e134b63a1f8a8ba0c942d552608ed7 # master`
- Pinned `actions/checkout@v2` → `@ee0669bd1cc54295c223e0bb666b733df41de1c5 # v2`
- Pinned `actions/upload-artifact@v2` → `@82c141cc518b40d92cc801eee768e7aafc9c2fa2 # v2`

**integration.yml**:
- Added `permissions: {}` at the top level
- Pinned `actions/checkout@v2` → `@ee0669bd1cc54295c223e0bb666b733df41de1c5 # v2`
- Pinned `Cyb3r-Jak3/html5validator-action@master` → `@443b108eb8e134b63a1f8a8ba0c942d552608ed7 # master`

