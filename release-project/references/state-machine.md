# Release State Machine

## States

| State | Evidence | Next action |
|---|---|---|
| `audited` | baseline, scope, target version, workflow, risks known | request Gate A approval when ready |
| `audited-not-ready` | audit complete but dirty scope, version mismatch, missing auth, or another prerequisite prevents preparation/publication | report exact remediation |
| `prepared` | changelog/version files ready; checks pass | request Gate B approval |
| `published-tag` | remote tag resolves to expected release commit | find or trigger workflow |
| `building` | matching workflow is queued or running | monitor without retriggering |
| `release-created` | GitHub Release exists for the exact tag | finalize notes and verify assets |
| `partial` | some artifacts or metadata exist | repair only missing step |
| `complete` | version, tag, Release notes, and assets verified | report completion |
| `blocked` | the requested next operation cannot proceed because approval, auth, permission, secrets, or checks are missing | report exact unblock action |

## Checkpoint Detection

Inspect rather than assume:

```text
Local version files -> expected target
Local tag -> exact commit
Remote tag -> exact commit
Workflow run -> tag/ref and conclusion
GitHub Release -> tag, title, body, state
Release assets -> names, sizes, timestamps
```

When evidence conflicts, stop and explain which source differs. Do not force one source to match another without explicit approval.

## Approval Boundaries

Gate A covers local release mutations only. Gate B covers remote state changes. Approval for one gate does not imply approval for the other.

Always reconfirm when the proposed target version, release scope, remote tag, or publication path changes after approval.

## Idempotency

- Reuse an existing correct local release change.
- Reuse an existing correct remote tag.
- Reuse and edit an existing GitHub Release.
- Monitor an existing workflow run.
- Rerun only a failed or missing operation after diagnosing it.
- Never create a second Release for the same tag.
