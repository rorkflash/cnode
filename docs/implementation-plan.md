# Cnode implementation plan

| ID | Work | Depends on | Completion evidence |
| --- | --- | --- | --- |
| C01 | Foreground daemon, CLI, configuration and SQLite journal | — | Single daemon/profile; durable state survives restart; status excludes secrets. |
| C02 | Enrollment, protocol client, long poll, receipts and event replay | C01, protocol | Fake Anode delivers duplicates and gaps; correct durable receipts and replay. |
| C03 | Supervisor, fake harness, launch intent, cancellation and recovery | C02 | Crash windows do not cause blind relaunch; stop uncertainty remains explicit. |
| C04 | Workspace and artifact preparation | C03 | Exact base verified; malicious paths rejected; partial upload recovery. |
| C05 | One real harness adapter | C03–C04 | Verified capability report, task output, cancellation and recovery on reference OS. |
| C06 | Talo integration and tests.diff | C05 | CLI/MCP tool call works; local result stays scoped; evidence export works; model absence harmless to exact helper. |
| C07 | Pinned package installation | C06 | Same approved helper on a second machine; hash/compatibility/permission denial tested. |
| C08 | External-job isolation and owner controls | C04–C07 | Enforced mounts/network/resources; no unrelated secrets; owner pause/cancel works. |
| C09 | User-service packaging and diagnostics | C08 | Restart/suspend/resume drill, bounded disk usage, upgrade preserves pending events. |
| C10 | Additional harnesses and OS platforms | C09 | Separate support report per adapter/version/OS; no silent capability fallback. |

## Integration milestone

Coordinate with Anode A03 using fake peers first. At Anode A07, demonstrate a task assigned on machine A, executed on machine B, with structured outcome and exact source evidence returned for review. Then put machine B to sleep or disconnect it and demonstrate the configured queue/replacement policy without stale accepted mutations.

The initial local owner may run trusted self-owned tasks before external-job isolation is complete. Shared work from another owner stays disabled until C08. This distinction is an execution policy, not a claim that all local code is harmless.

## Test strategy

Prioritize crash/restart, command duplication, per-run event order, lease expiry, adapter cancellation, safe extraction, disk pressure and capability negotiation. A mock process that always exits successfully is insufficient: test stuck processes, malformed output and a child that survives a failed stop.

Talo benchmarking is separate from protocol tests. Measure useful task success and total model tokens including inspection/retries. Do not present smaller prompts as proven savings without end-to-end evidence.

## Repository handoff prompt

“Read README.md, AGENTS.md, docs/architecture.md, docs/execution-and-recovery.md and docs/protocol.md. Implement C01 only. Keep transport and harness execution behind interfaces. Preserve journal data and use a fake adapter for later recovery tests before connecting a real model.”
