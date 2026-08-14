<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.0.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **KittyCAD--action-install-cli/v0.0.5** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.action_path }}` is directly interpolated inside a `run:` shell command in action.yml. Any `${{ ... }}` expression is substituted by the Actions runner before the shell sees the string, making it a script-injection vector. The offending line is: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`. This should be replaced with the safe env-var equivalent `$GITHUB_ACTION_PATH`.

Locations:

- `action.yml:14`

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ steps.install-kittycad.outputs.version }}` is directly interpolated inside a `run:` shell command in the workflow file. The offending line is: `echo "Version from output is: ${{ steps.install-kittycad.outputs.version }}"`. Step outputs can be attacker-influenced and must not be interpolated directly into shell commands. Use an `env:` variable and reference it as `"$ENV_VAR"` instead.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:16`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v3` (a mutable tag) in two separate jobs. Mutable tags can be moved to point to different — potentially malicious — commits. Each `uses:` reference should be pinned to a full 40-character commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`).

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:12`
- `.github/workflows/install-kittycad-cli-test.yml:22`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/install-kittycad-cli-test.yml` has no top-level `permissions:` key and neither of its jobs (`test-install-cli`, `convert-with-powershell`) defines a `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad. A minimal `permissions:` block (e.g. `contents: read`) should be added.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 findings:
1. action.yml script-injection: Replaced `${{ github.action_path }}` with `$GITHUB_ACTION_PATH` (the safe built-in env var) in the Linux install step.
2. Workflow script-injection: Moved `${{ steps.install-kittycad.outputs.version }}` into an `env:` block as `INSTALL_VERSION` and referenced it as `$INSTALL_VERSION` in the run script.
3. Workflow unpinned-uses: Pinned both `actions/checkout@v3` references to full SHA `f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3`.
4. Workflow missing-permissions: Added top-level `permissions: contents: read` block to the workflow file.

