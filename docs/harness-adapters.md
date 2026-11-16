# Harness adapter contract

Every harness has an explicit adapter. A command-line executable being installed is not proof it supports safe automation, session resume, headless access or shared subscription capacity.

## Adapter interface

| Operation | Required contract |
| --- | --- |
| `probe()` | Installed executable identity, version, OS, authentication readiness and verified capability set. Never expose secrets. |
| `prepare(run_context)` | Validate mode, working directory, tools, policy, input limits and required capabilities. |
| `start(run_context)` | Launch supervised work; return process identity and optional provider session ID. |
| `events(handle)` | Normalize progress, questions, usage and exit information into bounded structured events. |
| `follow_up(handle, message, expected_revision)` | Serialized input into a supported live session. Optional capability. |
| `resume(session_ref, context)` | Explicitly supported continuation, pinned to adapter version and correct workspace. Optional capability. |
| `cancel(handle, grace)` | Stop request and confirmation status; report unsupported/uncertain cases honestly. |
| `collect(handle)` | Outcome, source changes, evidence and usage refs; nonzero exit is not success. |
| `recover(handle)` | Validate process identity/session after restart; return recoverable, ended or unknown. |

Proposed capability keys: `headless_start`, `structured_output`, `follow_up`, `resume`, `cancel`, `usage_reporting`, `mcp_tools`, `gateway_endpoint`, `workspace_restriction`. Advertise each only after verification for the exact adapter/harness version. Partial support is acceptable if the scheduler excludes unsupported tasks.

## Candidate adapters

| Harness family | Planned integration stance |
| --- | --- |
| Claude Code | Candidate; verify supported invocation, authentication, tool permissions and session behavior before enablement. |
| Codex | Candidate; verify current supported machine interface and credential model before enablement. |
| Cursor | Candidate; distinguish headless/SDK capability from an interactive desktop session. |
| Antigravity | Candidate; verify allowed automation and authentication; remain disabled if unsupported. |
| OpenCode | Candidate; verify provider configuration, events and tool binding. |
| GitHub Copilot | Candidate; distinguish a supported harness integration from raw model access. |

This table is a backlog, not a compatibility matrix. No candidate is certified by this documentation. Recheck official documentation and terms when implementing; do not rely on undocumented OAuth extraction or desktop subscription forwarding.

## Invocation rules

- Spawn using an argument array; do not concatenate task text into shell commands.
- Use a run-specific working directory and environment allowlist.
- Supply credentials through the approved secret channel and scope them to the process.
- Bound stdin size, output buffering, runtime and child-process count.
- Redact credential-bearing output before logs or artifacts.
- Preserve raw output as an artifact only when policy permits; normalize a concise result.
- Never claim `workspace_restriction` from a prompt instruction alone.

## Lead and worker use

An adapter can serve either role; Anode supplies role instructions and scoped tools. A lead needs a reliable way to consume messages and call project operations. A worker needs a bounded task context and output contract. A role change creates a fresh role context or supported session transition; it does not transfer provider credentials between machines.

## Verification checklist per adapter release

Record version/OS, authorized authentication method, noninteractive launch, deterministic fixture completion, cancellation, process recovery, usage availability, output parsing and tool binding. Exercise an expired credential, malformed output, unavailable model, long output and a blocked user-input prompt. Freeze the report with the adapter release. Unsupported functionality must fail explicitly.

Start with a fake adapter that emits predefined events and writes a known patch. It is for protocol and recovery validation, not a benchmark of model quality. Enable one real adapter only after those tests pass.
