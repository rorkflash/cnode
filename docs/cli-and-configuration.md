# CLI, configuration and deployment

All commands below are proposed. No executable or package installation is supplied by this documentation starter.

## Owner CLI

| Proposed command | Responsibility |
| --- | --- |
| `cnode enroll` | Connect to an Anode origin with an owner-issued enrollment grant. Read secrets via protected input. |
| `cnode serve` | Run the per-user daemon in the foreground. |
| `cnode status --json` | Connection, capabilities, slots, runs, backlog and health; no secrets. |
| `cnode pause` / `cnode resume` | Pause/resume accepting new work. Existing jobs follow explicit policy. |
| `cnode runs list` / `cnode runs inspect <id>` | Local execution state and bounded evidence. |
| `cnode runs cancel <id>` | Owner stop request with confirmed/uncertain outcome. |
| `cnode adapters probe` | Verify installed adapter capabilities and readiness. |
| `cnode packages list` / `cnode packages install <id>@<version>` | Review and install pinned capabilities under local policy. |
| `cnode talo status` | Inspect configured Talo connection and capabilities. |
| `cnode doctor` | Check directories, credentials, connectivity and supported runtime dependencies. |

Machine callers get JSON on stdout and diagnostics on stderr. Nonzero exit indicates failure; partial results are explicitly marked. Do not include raw credentials in CLI arguments or diagnostics.

## Configuration

[Example configuration](../examples/cnode.config.example.yaml) documents the proposed shape. It contains placeholders and disabled external execution. Before implementing a loader, specify validation, unknown-field handling and the difference between defaults and owner decisions.

Policy precedence: OS restrictions + local owner policy + Anode execution grant + task-specific policy. Effective authority is their intersection. The cloud may tighten restrictions but cannot widen owner permissions. Detect impossible combinations before launch.

## OS integration

The initial reference platform is macOS, one service per logged-in OS user. Plan a user service through the OS service manager after foreground execution/recovery passes. Linux service packaging follows; Windows requires a separately tested process and isolation implementation. Do not advertise parity before validation.

Use platform-appropriate application-data, runtime-socket, log and credential-store locations. A user-only Unix socket is the first local transport. Never expose an unauthenticated local HTTP endpoint to the network.

## Local-only and cloud modes

Local-only: run local Anode with SQLite/filesystem artifacts and point Cnode at it. Shared: point Cnode at the cloud Anode HTTPS origin. Both use the same project/run contract. Migration is an Anode operation; switching an origin does not make two project databases merge.

## Upgrades

Pause new work, preserve pending events, drain or explicitly reconcile active runs, back up local state, apply compatible migrations and restart. Do not auto-upgrade an adapter underneath a running session. Keep rollback possible only when database compatibility permits it. Negotiate protocol before resuming assignments.

## Operational signals

Expose connection age, oldest undelivered event, active slots, lease time remaining, failed renewals, disk usage, adapter errors, stop-unconfirmed runs and Talo queue/health. A green daemon health check does not mean every installed harness is usable.
