<!-- markdownlint-disable -->

# Hardening Report: chrisreddington--validate-file-exists/v0.0.6

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **chrisreddington--validate-file-exists/v0.0.6** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: A ${{ }} expression is directly interpolated inside a run: shell command. In ci.yml line 54, `run: echo "${{ steps.test-action.outputs.exists }}"` passes a steps output value through YAML template substitution before the shell processes it. If the action output contains shell metacharacters, this can lead to command injection.

Locations:

- `.github/workflows/ci.yml:54`

### unpinned-uses (severity: high)

All uses: references across all workflow files use mutable tags or branch names instead of pinned 40-character SHA digests, making the workflows vulnerable to supply-chain attacks if the referenced action or workflow is compromised or altered. Failing references include: baseline.yml: `chrisreddington/reusable-workflows/.github/workflows/baseline-validator.yml@main`; check-dist.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `actions/upload-artifact@v4`; ci.yml: `actions/checkout@v4` (×2), `actions/setup-node@v4`; codeql-analysis.yml: `actions/checkout@v4`, `github/codeql-action/init@v3`, `github/codeql-action/autobuild@v3`, `github/codeql-action/analyze@v3`; linter.yml: `actions/checkout@v4`, `actions/setup-node@v4`, `super-linter/super-linter/slim@v7`.

Locations:

- `.github/workflows/baseline.yml:15`
- `.github/workflows/check-dist.yml:25`
- `.github/workflows/check-dist.yml:31`
- `.github/workflows/check-dist.yml:57`
- `.github/workflows/ci.yml:22`
- `.github/workflows/ci.yml:27`
- `.github/workflows/ci.yml:46`
- `.github/workflows/codeql-analysis.yml:27`
- `.github/workflows/codeql-analysis.yml:33`
- `.github/workflows/codeql-analysis.yml:38`
- `.github/workflows/codeql-analysis.yml:42`
- `.github/workflows/linter.yml:24`
- `.github/workflows/linter.yml:30`
- `.github/workflows/linter.yml:42`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed script injection in ci.yml by moving `${{ steps.test-action.outputs.exists }}` into an env: block (ACTION_OUTPUT) and referencing it as $ACTION_OUTPUT in the shell. Pinned all 14 unpinned action references across 5 workflow files to full 40-character SHA digests: actions/checkout@v4→11d5960a, actions/setup-node@v4→49933ea5, actions/upload-artifact@v4→ea165f8d, github/codeql-action/{init,autobuild,analyze}@v3→08d09a53, super-linter/super-linter/slim@v7→12150456, chrisreddington/reusable-workflows@main→7f52a20f. Original tags preserved as inline comments.

