<!-- markdownlint-disable -->

# Hardening Report: KittyCAD--action-install-cli/v0.2.21

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **KittyCAD--action-install-cli/v0.2.21** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression `${{ github.action_path }}` is directly interpolated inside a `run:` shell command string on line 19 of action.yml. The offending line is: `run: sudo --preserve-env ${{ github.action_path }}/entrypoint.sh`. Any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell ever sees it, making it a script-injection risk. The safe alternative is to use the pre-set environment variable `$GITHUB_ACTION_PATH` instead (as the Windows step already does correctly on line 25).

Locations:

- `action.yml:19`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Replaced `${{ github.action_path }}` with `"$GITHUB_ACTION_PATH"` in the Linux step's `run:` command (line 19 of action.yml). The GitHub Actions expression was being interpolated at the YAML template level before the shell saw it, creating a script injection risk. Using the pre-set environment variable `$GITHUB_ACTION_PATH` (which GitHub Actions sets automatically) is the safe alternative — it is resolved by the shell at runtime, not by the YAML templating engine. The path is also now quoted to handle any spaces. The Windows step was already using this pattern correctly and required no changes.

