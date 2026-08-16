<!-- markdownlint-disable -->

# Hardening Report: chrisreddington--validate-file-exists/v0.0.7

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **chrisreddington--validate-file-exists/v0.0.7** was hardened automatically. 7 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ steps.test-action.outputs.exists }}` is interpolated directly inside a `run:` shell command string. The `steps.*.outputs.*` context is workflow-controllable and flows through YAML template substitution before the shell sees it, enabling script injection. The offending line is: `run: echo "${{ steps.test-action.outputs.exists }}"`

Locations:

- `.github/workflows/ci.yml:57`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags or branch names instead of full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks. Failing references in baseline.yml: `chrisreddington/reusable-workflows/.github/workflows/baseline-validator.yml@main`.

Locations:

- `.github/workflows/baseline.yml:15`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of full 40-character SHA commit hashes. Failing references in check-dist.yml: `actions/checkout@v4` (line 25), `actions/setup-node@v4` (line 31), `actions/upload-artifact@v4` (line 62).

Locations:

- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:31`
- `.github/workflows/check-dist.yml:62`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of full 40-character SHA commit hashes. Failing references in ci.yml: `actions/checkout@v4` (lines 22, 49), `actions/setup-node@v4` (line 26).

Locations:

- `.github/workflows/ci.yml:22`
- `.github/workflows/ci.yml:26`
- `.github/workflows/ci.yml:49`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of full 40-character SHA commit hashes. Failing references in codeql-analysis.yml: `actions/checkout@v4` (line 24), `github/codeql-action/init@v3` (line 29), `github/codeql-action/autobuild@v3` (line 35), `github/codeql-action/analyze@v3` (line 40).

Locations:

- `.github/workflows/codeql-analysis.yml:24`
- `.github/workflows/codeql-analysis.yml:29`
- `.github/workflows/codeql-analysis.yml:35`
- `.github/workflows/codeql-analysis.yml:40`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of full 40-character SHA commit hashes. Failing references in copilot-setup-steps.yml: `actions/checkout@v4` (line 34), `actions/setup-node@v4` (line 37).

Locations:

- `.github/workflows/copilot-setup-steps.yml:34`
- `.github/workflows/copilot-setup-steps.yml:37`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tags instead of full 40-character SHA commit hashes. Failing references in linter.yml: `actions/checkout@v4` (line 18), `actions/setup-node@v4` (line 24), `super-linter/super-linter/slim@v7` (line 33).

Locations:

- `.github/workflows/linter.yml:18`
- `.github/workflows/linter.yml:24`
- `.github/workflows/linter.yml:33`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed all 7 findings across 6 workflow files:
1. ci.yml: Moved `${{ steps.test-action.outputs.exists }}` to env block (ACTION_EXISTS) to prevent script injection; pinned actions/checkout@v4 and actions/setup-node@v4 to full SHAs.
2. baseline.yml: Pinned chrisreddington/reusable-workflows@main to SHA 5190da866dc0991e7165aa6cc05020519dead9d9.
3. check-dist.yml: Pinned actions/checkout@v4, actions/setup-node@v4, and actions/upload-artifact@v4 to full SHAs.
4. codeql-analysis.yml: Pinned actions/checkout@v4, github/codeql-action/init@v3, github/codeql-action/autobuild@v3, and github/codeql-action/analyze@v3 to full SHAs.
5. copilot-setup-steps.yml: Pinned actions/checkout@v4 and actions/setup-node@v4 to full SHAs.
6. linter.yml: Pinned actions/checkout@v4, actions/setup-node@v4, and super-linter/super-linter/slim@v7 to full SHAs.
All mutable tag references replaced with immutable commit SHAs while preserving the original tag in a comment for readability.

