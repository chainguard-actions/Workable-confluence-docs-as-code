<!-- markdownlint-disable -->

# Hardening Report: Workable--confluence-docs-as-code/v1.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Workable--confluence-docs-as-code/v1.6.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable tag refs instead of full 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the upstream repository is compromised.

Failing references:
- .github/workflows/node.ci.yml: `actions/checkout@v4`, `actions/setup-node@v4`
- .github/workflows/semver.yml: `actions/checkout@v4`, `simbo/action-semver-release-action@v1`

Each should be pinned to a full SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/node.ci.yml:17`
- `.github/workflows/node.ci.yml:19`
- `.github/workflows/semver.yml:12`
- `.github/workflows/semver.yml:14`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` block, and neither job within them defines job-level permissions. Without explicit permissions, workflows run with the default token permissions, which may be broader than necessary (e.g. write access to contents and pull-requests). A minimal explicit permissions block (e.g. `permissions: read-all` or specific scopes) should be added.

Locations:

- `.github/workflows/node.ci.yml:1`
- `.github/workflows/semver.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

**node.ci.yml**:
- Pinned `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`
- Pinned `actions/setup-node@v4` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
- Added `permissions: contents: read` (minimal for a CI build/test workflow)

**semver.yml**:
- Pinned `actions/checkout@v4` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4`
- Pinned `simbo/action-semver-release-action@v1` → `simbo/action-semver-release-action@f77f66747dc75fc5f069e34545f908ee14299216 # v1`
- Added `permissions: contents: write` (required for the semver release action to create/update git tags)

