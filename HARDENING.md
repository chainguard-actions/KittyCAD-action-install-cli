<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **KittyCAD--action-install-cli/v0.1.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.action_path }}` is directly interpolated inside a `run:` shell command string on line 19 of action.yml. The offending line is: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`. Even though `github.action_path` is typically GitHub-controlled, any `${{ ... }}` expression interpolated directly into a `run:` block undergoes YAML template substitution before the shell sees it, bypassing shell quoting and enabling script injection. The safe alternative is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead (as is already done correctly in the Windows step on line 25).

Locations:

- `action.yml:19`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Replaced `${{ github.action_path }}` with `"$GITHUB_ACTION_PATH"` in the Linux install step's `run:` command (action.yml line 19). The expression was being interpolated via YAML template substitution before the shell processed it, bypassing shell quoting. Using the pre-set `$GITHUB_ACTION_PATH` environment variable (double-quoted) is the safe alternative, consistent with how the Windows step already handled it.

