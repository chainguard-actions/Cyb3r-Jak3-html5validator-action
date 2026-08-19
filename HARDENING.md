<!-- markdownlint-disable -->

# Hardening Report: Cyb3r-Jak3--html5validator-action/v8.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Cyb3r-Jak3--html5validator-action/v8.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Rule (b) violation: In entrypoint.sh line 50, the shell variables `${BuildARGS}` and `${INPUT_EXTRA}` are expanded **unquoted** inside the `html5validator` command invocation. `INPUT_EXTRA` maps directly to the user-controlled `inputs.extra` action input, and `BuildARGS` is constructed from `INPUT_FORMAT` (`inputs.format`) and `INPUT_BLACKLIST` (`inputs.blacklist`) — also user-controlled. Unquoted expansion allows an attacker to inject shell metacharacters (`;`, `|`, `&`, `$(...)`, backticks, glob patterns, etc.) via these inputs, achieving arbitrary command execution inside the Docker container. The offending line is: `html5validator --root "${INPUT_ROOT}" --log "${INPUT_LOG_LEVEL}" ${BuildARGS} ${INPUT_EXTRA} |& tee log.log`. These variables must be double-quoted: `"${BuildARGS}"` and `"${INPUT_EXTRA}"`.

Locations:

- `entrypoint.sh:50`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in entrypoint.sh line 50. Converted BuildARGS from an unquoted string variable to a bash array, with each argument (flag + value) added as separate array elements. Added ExtraARGS array that safely tokenizes INPUT_EXTRA using xargs (quote-aware splitting) with a guard for empty input. The html5validator command now uses "${BuildARGS[@]}" and "${ExtraARGS[@]}" for safe, properly quoted array expansion, preventing shell metacharacter injection via user-controlled inputs.

