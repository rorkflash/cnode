# Development instructions for Cnode

This is a documentation-first repository. Read README.md, docs/architecture.md,
docs/protocol.md and docs/implementation-plan.md before implementation. Proposed
commands and paths are not evidence of existing functionality.

## Boundaries

- Anode owns shared project authority, task/history state, grants and leases.
- Cnode owns local processes, workspaces, owner policy and durable replay.
- Talo remains independently versioned; integrate its API instead of copying its engine.
- Do not fabricate provider capabilities or promise subscription redistribution.
- Keep task completion, result receipt, review acceptance and code integration distinct.

## Implementation discipline

Start with the earliest incomplete ticket. Keep changes reviewable and document
any changed contract. Use fake peers and fake harnesses before invoking a paid
provider. Choose and pin dependencies deliberately; record supported versions.

Never treat a prompt or a model decision as permission, budget enforcement or a
process-isolation boundary. Enforce scope in code and at the actual side effect.
Persist before acknowledging. Retain unknown state instead of claiming success.

## Shared protocol

The protocol is vendored from Anode. Propose changes upstream; do not edit the mirrored contract independently. Update the complete pinned release and integrity manifest together.

## Verification and reporting

Test meaningful domain, contract and failure-path behavior appropriate to the
change. Do not write tests that only restate implementation details. Include
duplicate delivery, stale authority and recovery when modifying execution.
Report actual commands/checks run and limitations. Do not claim the application
starts until an executable implementation exists and has been exercised.

## Public repository hygiene

Keep credentials, personal paths, source bundles and user conversations out of
commits. Use synthetic fixtures. Do not publish a package or create an external
message as a side effect of running documentation examples. License selection
remains an owner decision.
