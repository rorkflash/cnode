# Local security and owner control

## Trust boundaries

The Anode operator, project owner, machine owner, harness provider and package publisher can be different parties. The owner grants machine capacity; the project owner grants data access. Neither grant implies the other.

| Boundary | Enforced control |
| --- | --- |
| Cloud → local machine | Enrolled identity, protocol validation and local policy before launch. |
| Harness → filesystem | Approved workspace/mounts with OS enforcement for external jobs. |
| Harness → network | Explicit destination policy and credential scope. |
| Harness → Talo | User/workspace-bound socket and result references. |
| Package → execution | Pinned hash, compatibility, trust policy and isolation. |
| Worker → repository publication | Separate authorized connector and current lease checks. |

External jobs remain disabled until an isolation mechanism is selected and tested on the target OS. A worktree, container label or prompt is not evidence of containment by itself; verify actual mounts, process escape, network and secret exposure.

## Secrets

Store runner credentials in the OS credential store. Provider-native authentication stays with its supported harness flow and owner. Pass short-lived gateway credentials only to the intended process. Never export cookies, OAuth refresh tokens or another user's desktop session to Anode.

Keep Git publication credentials outside worker execution where possible. Redact diagnostics before upload. Credential revocation prevents new access; it cannot remove previously delivered source or undo completed operations.

## Untrusted input

Treat repository instructions, shared skills, task messages and worker outputs as untrusted data relative to privilege decisions. Validate schemas and paths. Spawn argument arrays, not shell strings constructed from task text. Bound output, input, archive expansion and dependency installation.

## Owner controls

The owner can pause new jobs, revoke contribution, stop a run, inspect local activity and disconnect a profile. Anode cannot bypass those controls. Report whether cancellation actually stopped the process. If control is uncertain, retain evidence and surface the unresolved state.

## Release gate

Before enabling external tasks, demonstrate no access outside approved mounts, no exposure of unrelated credentials, safe archive extraction, bounded resources, cross-workspace Talo reference denial, and graceful behavior under disk exhaustion. Publish supported limitations per OS and adapter.
