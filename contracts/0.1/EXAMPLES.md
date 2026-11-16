# Synthetic protocol examples

`assignment.json` is a mediated Git task. `run-started.json` and `run-result.json`
are contiguous events for that assignment. Hashes, refs and IDs are synthetic;
there are no downloadable artifacts or actual repository commits behind them.

Validate using a JSON Schema draft 2020-12 validator with date-time format checking.
Schema validation does not replace runtime authorization or lifecycle checks.

Additional conformance cases to implement:

- Replay assignment unchanged: original receipt, no second launch.
- Reuse its command ID with a changed task: idempotency conflict.
- Send result sequence 2 before start sequence 1: sequence gap.
- Replay the accepted result: original receipt, no duplicate outcome.
- Send the result after authority replacement: late quarantine.
- Alter project/runner identity: permission denied before processing.
- Change artifact hash or omit finalization: result cannot reference it as valid evidence.
- Add an unnegotiated field or event type: validation/version failure.
- Report stop-unconfirmed as run.cancelled: reject; cancellation requires confirmation.

Timestamp ordering, deadline/lease bounds, repository ancestry, package compatibility,
artifact ownership and task authority require semantic checks outside JSON Schema.
