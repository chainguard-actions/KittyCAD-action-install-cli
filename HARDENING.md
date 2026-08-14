<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.2.12

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **KittyCAD--action-install-cli/v0.2.12** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The 'install cli Linux' step in action.yml directly interpolates `${{ github.action_path }}` inside a `run:` shell command string. Any `${{ ... }}` expression inside a run: block is a script-injection risk because the value is substituted by the YAML template engine before the shell ever sees it. Offending line: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`

Locations:

- `action.yml:19`

### script-injection (severity: high)

Sub-rule (a): The 'check-install' step in the workflow directly interpolates `${{ steps.install-kittycad.outputs.version }}` inside a `run:` shell command string (an echo statement). Step outputs are workflow-controllable and must not be interpolated directly into shell commands. Offending line: `echo "Version from output is: ${{ steps.install-kittycad.outputs.version }}"`

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:17`

### unpinned-uses (severity: high)

Both jobs in the workflow use `actions/checkout@v3`, which is a mutable tag reference rather than a pinned 40-character commit SHA. This is vulnerable to supply-chain attacks if the tag is moved. Failing references: `actions/checkout@v3` (line 13, job test-install-cli) and `actions/checkout@v3` (line 24, job convert-with-powershell).

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:13`
- `.github/workflows/install-kittycad-cli-test.yml:24`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key, and neither job (`test-install-cli` nor `convert-with-powershell`) defines its own `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed 4 findings across 2 files:
1. action.yml (script-injection): Moved `${{ github.action_path }}` out of the run: shell string into an env: block (ACTION_PATH), referenced as "$ACTION_PATH/entrypoint.sh" in the shell.
2. .github/workflows/install-kittycad-cli-test.yml (script-injection): Moved `${{ steps.install-kittycad.outputs.version }}` into an env: block (INSTALL_VERSION), referenced as $INSTALL_VERSION in the echo command.
3. .github/workflows/install-kittycad-cli-test.yml (unpinned-uses): Pinned both `actions/checkout@v3` references to full SHA `f43a0e5ff2bd294095638e18286ca9a3d1956744` with `# v3` comment.
4. .github/workflows/install-kittycad-cli-test.yml (missing-permissions): Added `permissions: {}` at the top level to explicitly restrict all token permissions.

