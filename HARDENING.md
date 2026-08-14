<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **KittyCAD--action-install-cli/v0.1.1** was hardened automatically. 4 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (a) violation: A ${{ }} expression is interpolated directly inside a `run:` shell command string. In action.yml line 19, `${{ github.action_path }}` is embedded directly in the shell command `sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`. Any ${{ ... }} expression in a run: block is a script-injection risk because the value is substituted into the shell command string before the shell parses it.

Locations:

- `action.yml:19`

### script-injection (severity: high)

Rule (a) violation: A ${{ }} expression is interpolated directly inside a `run:` shell command string in the workflow. On line 18 of install-kittycad-cli-test.yml, `${{ steps.install-kittycad.outputs.version }}` is embedded directly in `echo "Version from output is: ${{ steps.install-kittycad.outputs.version }}"`. Step outputs can contain attacker-influenced content (e.g. from a prior step that processed PR content), and direct interpolation allows shell metacharacter injection.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:18`

### unpinned-uses (severity: high)

The workflow uses `actions/checkout@v3` (a mutable tag reference) in two jobs instead of a pinned full 40-character commit SHA. Mutable tags can be moved by the upstream repository owner or an attacker who compromises it, enabling supply-chain attacks. Failing references: `actions/checkout@v3` at lines 13 and 23.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:13`
- `.github/workflows/install-kittycad-cli-test.yml:23`

### missing-permissions (severity: medium)

The workflow file has no top-level `permissions:` key and neither of its jobs (`test-install-cli`, `convert-with-powershell`) defines a `permissions:` block. Without explicit permissions, the workflow inherits the repository's default token permissions, which may be overly broad (e.g. write access to contents). Explicit minimal permissions should be declared.

Locations:

- `.github/workflows/install-kittycad-cli-test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all four findings: (1) action.yml line 19 - moved ${{ github.action_path }} into env block as ACTION_PATH, referenced as "$ACTION_PATH/entrypoint.sh" in the run command; (2) workflow line 18 - moved ${{ steps.install-kittycad.outputs.version }} into env block as INSTALLED_VERSION, referenced as $INSTALLED_VERSION in the echo command; (3) workflow lines 13 and 23 - pinned both actions/checkout@v3 references to full SHA f43a0e5ff2bd294095638e18286ca9a3d1956744 with # v3 comment; (4) workflow line 1 - added top-level permissions: {} to explicitly restrict token permissions.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed the 'install cli Windows' step in action.yml: moved GITHUB_ACTION_PATH into an env block as ACTION_PATH=${{ github.action_path }} (consistent with the Linux step pattern) and double-quoted the variable expansion in the run command: "$ACTION_PATH/entrypoint-win.sh". This prevents shell metacharacter injection from the workflow-controllable GITHUB_ACTION_PATH environment variable.

