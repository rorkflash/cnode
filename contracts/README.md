# Protocol contract ownership

Contract: **draft 0.1**. Canonical owner: **Anode repository**.
This is a vendored snapshot; changes originate in Anode.

[Protocol prose](../docs/protocol.md) defines behavior. `0.1/` contains JSON Schema
for assignment and events, with synthetic examples. These are proposed starter
contracts, not a complete server OpenAPI specification. Enrollment, cancellation,
follow-up, receipts and errors still need full endpoint schemas before release.

`manifest.json` pins the SHA-256 of each shared schema, fixture and protocol text.
Copy the complete contract release and protocol document into Cnode together.
Check hashes and validate fixtures in both repositories before merging an upgrade.
Do not reinterpret a stricter schema as allowing extra unknown wire fields.

Release process:

1. Propose behavior and version change in Anode with conformance examples.
2. Review both producer and consumer behavior, including old/new negotiation.
3. Publish immutable contract files and updated hash manifest.
4. Update Cnode's vendored files and verify them against the Anode release.
5. Enable the new version only after integration evidence exists.

A third protocol repository is unnecessary initially. Extract one later only if
additional independent implementations make it useful.
