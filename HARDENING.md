<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.2.12

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **KittyCAD--action-install-cli/v0.2.12** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.action_path }}` is interpolated directly inside a `run:` shell command string in action.yml. Any `${{ ... }}` expression inside a `run:` block is a script-injection risk because the value is substituted into the shell command before the shell parses it. The safe alternative is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead (as already done in the Windows step on line 25). Offending line: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`

Locations:

- `action.yml:19`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Replaced `${{ github.action_path }}` with `"$GITHUB_ACTION_PATH"` in the Linux install step's `run:` command (line 19 of action.yml). The pre-set environment variable `$GITHUB_ACTION_PATH` is the safe alternative — it is set by the runner before the shell executes, so it cannot be manipulated via expression injection. The path is now also double-quoted to handle any spaces in the path. This matches the pattern already used in the Windows step.

