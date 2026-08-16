<!-- markdownlint-disable -->

# Hardening Report: chrisreddington--validate-file-exists/v0.0.8

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **chrisreddington--validate-file-exists/v0.0.8** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

All workflow files reference GitHub Actions using mutable version tags instead of pinned full 40-character SHA commits. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code.

Failing references:
- baseline.yml: `uses: chrisreddington/reusable-workflows/.github/workflows/baseline-validator.yml@main`
- check-dist.yml: `uses: actions/checkout@v4`, `uses: actions/setup-node@v4`, `uses: actions/upload-artifact@v4`
- ci.yml: `uses: actions/checkout@v4`, `uses: actions/setup-node@v4`
- codeql-analysis.yml: `uses: actions/checkout@v4`, `uses: github/codeql-action/init@v3`, `uses: github/codeql-action/autobuild@v3`, `uses: github/codeql-action/analyze@v3`
- copilot-setup-steps.yml: `uses: actions/checkout@v4`, `uses: actions/setup-node@v4`
- linter.yml: `uses: actions/checkout@v4`, `uses: actions/setup-node@v4`, `uses: super-linter/super-linter/slim@v7`

All should be pinned to full SHA digests, e.g. `uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/baseline.yml:14`
- `.github/workflows/check-dist.yml:27`
- `.github/workflows/ci.yml:22`
- `.github/workflows/codeql-analysis.yml:27`
- `.github/workflows/copilot-setup-steps.yml:33`
- `.github/workflows/linter.yml:22`

### script-injection (severity: high)

Sub-rule (a) violation: A GitHub Actions expression `${{ steps.test-action.outputs.exists }}` is directly interpolated inside a `run:` shell command string. The expression `steps.*.outputs.*` flows through YAML template substitution before the shell processes it, allowing an attacker who can influence the action's output to inject arbitrary shell commands.

Offending line:
  `run: echo "${{ steps.test-action.outputs.exists }}"`

Fix: Move the value into an `env:` variable and reference it as a quoted shell variable:
```yaml
env:
  ACTION_OUTPUT: ${{ steps.test-action.outputs.exists }}
run: echo "$ACTION_OUTPUT"
```

Locations:

- `.github/workflows/ci.yml:57`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Pinned all unpinned action references across 6 workflow files to their full 40-character SHA commits (with original tags preserved as comments). Fixed script injection in ci.yml by moving `${{ steps.test-action.outputs.exists }}` into an env variable `ACTION_OUTPUT` and referencing it as `$ACTION_OUTPUT` in the run step.

### Iteration 2

**Fixes applied:** unpinned-uses

**Notes:**

Fixed all three unpinned-uses findings:
1. `.github/workflows/codeql-analysis.yml` line 33: Pinned `github/codeql-action/init@v3` → `@4187e74d05793876e9989daffde9c3e66b4acd07 # v3`
2. `.github/workflows/copilot-setup-steps.yml` line 33: Pinned `actions/setup-node@v4` → `@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
3. `.github/workflows/linter.yml` line 35: Cleaned up the malformed step (which had a corrupted name field containing an embedded `uses:` directive) and pinned `super-linter/super-linter/slim@v7` → `@12150456a73e248bdc94d0794898f94e23127c88 # v7`. All SHAs were resolved via lookup_action_sha.

