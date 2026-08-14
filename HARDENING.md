<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.2.16

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **KittyCAD--action-install-cli/v0.2.16** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a): A GitHub Actions expression `${{ github.action_path }}` is directly interpolated inside a `run:` shell command string. Even though `github.action_path` is GitHub-controlled, any `${{ ... }}` expression inside a `run:` block is a script-injection risk because the value flows through YAML template substitution before the shell ever sees it. The offending line is: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`. Fix: use the `$GITHUB_ACTION_PATH` environment variable instead (as already done in the Windows step).

Locations:

- `action.yml:16`

### script-injection (severity: high)

Rule (a): A GitHub Actions expression `${{ steps.install-kittycad.outputs.version }}` is directly interpolated inside a `run:` shell command string in the workflow. Step outputs can be attacker-influenced and are injected into the shell command before quoting. The offending line is: `echo "Version from output is: ${{ steps.install-kittycad.outputs.version }}"`  Fix: assign the value to an env var and reference it as `"$ENV_VAR"` in the shell.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:17`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v3` (a mutable tag reference) in two jobs. Tags can be moved to point to a different, potentially malicious commit. Both occurrences must be pinned to a full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`).

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:12`
- `.github/workflows/install-kittycad-cli-test.yml:22`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and no job-level `permissions:` key on any of its jobs (`test-install-cli`, `convert-with-powershell`). Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. Add a top-level `permissions: {}` block (or minimal specific scopes) to restrict the GITHUB_TOKEN.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed 4 findings across 2 files:
1. action.yml: Replaced `${{ github.action_path }}` with `$GITHUB_ACTION_PATH` in the Linux install step to eliminate script-injection risk.
2. .github/workflows/install-kittycad-cli-test.yml: (a) Added top-level `permissions: {}` block; (b) Pinned both `actions/checkout@v3` references to full SHA `a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3`; (c) Moved `${{ steps.install-kittycad.outputs.version }}` into an `env:` block as `INSTALLED_VERSION` and referenced it as `$INSTALLED_VERSION` in the shell script.

