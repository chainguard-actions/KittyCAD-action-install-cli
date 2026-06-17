<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.2.16

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **KittyCAD--action-install-cli/v0.2.16** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A `${{ ... }}` expression is interpolated directly inside a `run:` shell command string. In action.yml line 19, `${{ github.action_path }}` is embedded directly in the shell command: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`. Any `${{ ... }}` expression inside a `run:` block is a script-injection risk because the value is substituted into the shell command string before the shell parses it, bypassing shell quoting. The safe alternative is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead (as already done correctly in the Windows step on line 25).

Locations:

- `action.yml:19`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection on action.yml line 19 by replacing `${{ github.action_path }}` with the pre-set environment variable `$GITHUB_ACTION_PATH` (double-quoted for safety). This matches the pattern already correctly used in the Windows step on line 25. The expression `${{ github.action_path }}` was being interpolated directly into the shell command string before the shell parsed it, which is a script injection risk.

