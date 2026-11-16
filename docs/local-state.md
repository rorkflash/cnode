# Local state and persistence

Cnode uses SQLite on a local filesystem, with short transactions, a busy timeout and a bounded write queue. Large outputs live in an artifact spool. The server's project database is never mounted or written directly.

| Record | Purpose / invariant |
| --- | --- |
| profiles | Anode origin, runner identity and credential-store references; no plaintext token. |
| capabilities | Adapter version and last advertised capability revision. |
| commands | Unique command ID, content hash, payload, validation and acknowledgment state. |
| runs | Run/epoch, worker, pinned inputs, execution state and authority snapshot. |
| launch_intents | Unique launch token and handshake state to resolve crash windows. |
| process_handles | PID plus start identity, wrapper token and optional adapter session reference. |
| local_slots | Transactional local reservations; never exceed owner concurrency. |
| events | Immutable event ID, per-run sequence, body hash and delivery receipt. |
| delivery_state | Last durably persisted command cursor and contiguous event acknowledgments. |
| workspaces | Run-bound root, exact base, policy, cleanup state and retention pins. |
| transfers | Artifact hash/size, upload state, finalized ref and retry metadata. |
| package_installs | Immutable release/hash, approval, compatibility and current run references. |
| local_audit | Enrollment, owner policy changes, denied actions, cancellation and recovery. |

## Atomic operations

- Command insertion plus receipt creation precedes acknowledgment.
- Local slot reservation plus transition to preparing is one transaction.
- Event insertion plus next-sequence advancement is one transaction.
- Result event references only finalized uploads and records its durable submission state.
- Cleanup marks intent, checks references, removes files and records completion; recover interrupted cleanup safely.

Do not hold database transactions open during provider calls, Git operations or artifact uploads. Persist intent and reconcile external effects after interruption.

## Storage pressure

Set maximum journal, spool and workspace usage. Stop accepting new tasks before the filesystem is full. Never delete unacknowledged results to make room. Bound streamed logs and report truncation; overflow is an explicit run condition rather than an unbounded memory queue.

## Startup recovery

Acquire a per-profile daemon lock → open/migrate local state → validate process identities → inventory unresolved launch intents → connect/reconcile with Anode → resume only authorized work. A schema migration failure leaves the daemon read-only or stopped; it must not silently reset the journal.

Back up configuration and journal consistently with referenced artifacts when needed. Device secrets stay in the credential store and may require reenrollment after machine restoration. Do not copy a device identity to a second active machine.
