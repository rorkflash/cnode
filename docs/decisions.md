# Decisions and open choices

## Accepted product boundaries

- Cnode owns local execution; Anode owns shared coordination and project memory.
- Talo is integrated but remains independently usable and versioned.
- A machine owner controls contribution and can stop local work.
- The normal model path may go directly to Anode's gateway; Cnode is not a mandatory proxy.
- Shared capabilities are pinned packages, not dynamically trusted code from any project.

## Proposed technical baseline

| Decision | Rationale / consequence |
| --- | --- |
| Python per-user daemon | Matches the Talo starting point; a Rust implementation can follow the same contract. |
| macOS reference platform first | Matches Talo's existing local design; Linux and Windows need separate validation. |
| SQLite journal plus artifact spool | Durable replay without a local database service. |
| Explicit harness adapters | Provider capabilities differ; no assumed universal desktop control. |
| Local Anode for standalone mode | Avoid duplicating server domain logic in Cnode. |
| Separate Talo/model processes | Helper/model failure must not block heartbeat or journal. |
| One authority per profile | Prevent accidental mixing of projects, credentials and run IDs across hubs. |

## Open before external use

Choose first real harness and exact supported version, verified authentication mode, OS isolation mechanism, dependency installation policy, service packaging, owner notification surface and retention limits. Select a local model only after evaluation. Choose the software license before public release.

## Provenance

Derived from Agent Hub Architecture 1.0 and Talo Service Design. The command shapes and configuration are implementation proposals. Talo's proposed user-only socket and helper interfaces remain its own contract; Cnode should adapt rather than fork them.
