<!-- markdownlint-disable -->

# Hardening Report: chrisreddington--validate-file-exists/v0.0.9

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **chrisreddington--validate-file-exists/v0.0.9** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable tag or branch refs instead of pinned full-length commit SHAs. This exposes the workflow to supply-chain attacks where a tag or branch can be silently moved to point to malicious code.

Failing references:
- baseline.yml: `chrisreddington/reusable-workflows/.github/workflows/baseline-validator.yml@main` (branch ref)
- check-dist.yml: `actions/checkout@v5`, `actions/setup-node@v6`, `actions/upload-artifact@v5` (tag refs)
- ci.yml: `actions/checkout@v5`, `actions/setup-node@v6` (tag refs)
- codeql-analysis.yml: `actions/checkout@v5`, `github/codeql-action/init@v4`, `github/codeql-action/autobuild@v4`, `github/codeql-action/analyze@v4` (tag refs)
- copilot-setup-steps.yml: `actions/checkout@v5`, `actions/setup-node@v6` (tag refs)
- linter.yml: `actions/checkout@v5`, `actions/setup-node@v6` (tag refs)

Note: `super-linter/super-linter/slim@2bdd90ed3262e023ac84bf8fe35dc480721fc1f2` in linter.yml is correctly pinned.

Locations:

- `.github/workflows/baseline.yml:13`
- `.github/workflows/check-dist.yml:26`
- `.github/workflows/check-dist.yml:32`
- `.github/workflows/check-dist.yml:57`
- `.github/workflows/ci.yml:20`
- `.github/workflows/ci.yml:26`
- `.github/workflows/ci.yml:43`
- `.github/workflows/codeql-analysis.yml:27`
- `.github/workflows/codeql-analysis.yml:33`
- `.github/workflows/codeql-analysis.yml:39`
- `.github/workflows/codeql-analysis.yml:43`
- `.github/workflows/copilot-setup-steps.yml:30`
- `.github/workflows/copilot-setup-steps.yml:35`
- `.github/workflows/linter.yml:22`
- `.github/workflows/linter.yml:28`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned all mutable tag/branch action references to full commit SHAs across 6 workflow files:
- baseline.yml: chrisreddington/reusable-workflows@main → @7f52a20f6e1f14e5f5df25bc144ce2c462a61daf # main
- check-dist.yml: actions/checkout@v5 → @fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09 # v5; actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 # v6; actions/upload-artifact@v5 → @330a01c490aca151604b8cf639adc76d48f6c5d4 # v5
- ci.yml: both actions/checkout@v5 → @fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09 # v5; actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 # v6
- codeql-analysis.yml: actions/checkout@v5 → @fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09 # v5; github/codeql-action/{init,autobuild,analyze}@v4 → @7188fc363630916deb702c7fdcf4e481b751f97a # v4
- copilot-setup-steps.yml: actions/checkout@v5 → @fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09 # v5; actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 # v6
- linter.yml: actions/checkout@v5 → @fbc6f3992d24b796d5a048ff273f7fcc4a7b6c09 # v5; actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 # v6
The super-linter/super-linter/slim reference in linter.yml was already correctly pinned and left unchanged.

