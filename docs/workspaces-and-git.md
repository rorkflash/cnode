# Workspaces and source exchange

## Workspace ownership

Each run receives an isolated directory keyed by profile/project/run. Keep metadata outside agent-writable source where possible. Pin source commit, context manifest, package lock, adapter version and policy revision. Do not reuse another user's live working directory.

Worktrees can isolate edits for trusted local work. External or untrusted work requires an enforced execution boundary with controlled filesystem mounts, network and resources. The mechanism is OS-specific and must be selected and verified before external jobs are enabled.

## Source preparation

| Mode | Cnode behavior |
| --- | --- |
| `none` | Create a task workspace for documents or research; no repository assumptions. |
| `git_bundle` | Fetch authorized artifact, verify digest, inspect bundle, import permitted objects/refs and check out exact base. |
| `authorized_clone` | Use an explicitly approved credential source; fetch exact base into an isolated checkout. |

Resolve paths after symlinks; reject traversal, absolute extraction paths, device entries and links escaping the workspace. Cap archive expansion and object sizes. Do not execute repository hooks or package install scripts while merely importing untrusted source. Execution of dependencies belongs to a separately authorized preparation phase.

Verify repository identity and the actual base commit before launch. Submodules, LFS, private dependencies, toolchain versions and environment prerequisites must be declared. If a prerequisite is missing, return a preparation error rather than silently using a different source revision.

## Returning changes

Create local commits when the workflow permits; local commit creation does not require Git-host authentication. Record actual author/committer provenance and run attribution. Export a bundle or patch plus base/result refs, changed-path summary and evidence. Never include local secrets or unrelated files.

The server connector verifies history and publishes a task branch. Cnode's submission is evidence, not authority to merge. Contributor mode may allow a direct task-branch push, but only under explicit grants. Ref restrictions must be enforced by the connector/host, not merely requested in a prompt.

## Integration

Code review and integration may be assigned to a separate run. Test the exact candidate produced against the current target revision. If the target changes, a new integration attempt may be required. Independent tasks use separate branches/checkouts; dependency tasks use the accepted prerequisite commit.

## Cleanup

Retain workspace and outgoing artifacts until durable result receipt plus configured retention. Pin files needed by a pending retry, review or evidence export. Cleanup must consult journal references rather than deleting directories solely by age. Offer owner inspection/export before removing a failed workspace when policy permits. Record cleanup completion without retaining secrets.
