<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.0.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **KittyCAD--action-install-cli/v0.0.5** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a) violation: A GitHub Actions expression `${{ github.action_path }}` is interpolated directly inside a `run:` shell command string. Any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell ever sees it, making it a script-injection risk. The offending line is: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`. This should be replaced with the safe environment variable form `$GITHUB_ACTION_PATH` (which is already used correctly in the Windows step on line 24).

Locations:

- `action.yml:19`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Replaced `${{ github.action_path }}` with `"$GITHUB_ACTION_PATH"` in the Linux install step's `run:` command (line 19 of action.yml). The `$GITHUB_ACTION_PATH` environment variable is the safe equivalent that avoids YAML template substitution before shell execution. The path is also now properly double-quoted to handle any spaces. This matches the pattern already used correctly in the Windows step.

