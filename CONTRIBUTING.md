# Contributing

This repository begins with architecture and implementation contracts. Use the
implementation plan to select a bounded change and name its completion evidence.

For a design change, explain the concrete problem, resulting behavior, affected
protocol/version and migration implications. For code, include relevant checks
and observed limitations. Keep credentials and real project content out of fixtures.

Changes to shared wire semantics originate in Anode and update Cnode's pinned
contract. Application releases need not be simultaneous when the negotiated
protocol remains compatible. Never modify an existing released contract in place.

Security-sensitive failures should be reported privately through a maintainer
channel established before public launch; this starter does not invent an email
address. Do not post live secrets in an issue.

A software license must be selected before open-source release. This starter
deliberately does not assign licensing rights or contributor terms for the owner.
