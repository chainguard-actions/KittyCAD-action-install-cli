<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.2.21

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **KittyCAD--action-install-cli/v0.2.21** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): `${{ github.action_path }}` is directly interpolated inside a `run:` shell command string in action.yml. Even though `github.action_path` is not attacker-controlled in the same way as `github.head_ref`, any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell sees it and is a script-injection violation. The offending line is: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`. It should be replaced with the safe env-var form `$GITHUB_ACTION_PATH` (which is already used correctly in the Windows step).

Locations:

- `action.yml:17`

### script-injection (severity: high)

Sub-rule (a): `${{ steps.install-zoo.outputs.version }}` is directly interpolated inside a `run:` shell command string in the workflow. The offending line is: `echo "Version from output is: ${{ steps.install-zoo.outputs.version }}"`. Step outputs can contain attacker-influenced content and must not be interpolated directly into shell commands. Use an `env:` variable and reference it as `"$ENV_VAR"` instead.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:18`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v3` (a mutable tag reference) in two jobs. Tags can be moved to point to a different, potentially malicious commit. Each `uses:` reference should be pinned to a full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`). Failing references: `actions/checkout@v3` at lines 13 and 24.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:13`
- `.github/workflows/install-kittycad-cli-test.yml:24`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and neither of its jobs (`test-install-cli`, `convert-with-powershell`) defines a `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal `permissions:` block (e.g. `contents: read`) should be added at the top level or per job.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed four findings across two files: (1) action.yml line 17: replaced `${{ github.action_path }}` with `"$GITHUB_ACTION_PATH"` to eliminate template interpolation in run: shell command; (2) workflow line 18: moved `${{ steps.install-zoo.outputs.version }}` into an env: block as INSTALL_VERSION and referenced it as $INSTALL_VERSION in the shell script; (3) workflow lines 13 and 24: pinned both `actions/checkout@v3` to full SHA `a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3`; (4) workflow line 1: added top-level `permissions: contents: read` block.

