---
name: the-handoff-is-written-for-a-stranger
title: The handoff is written for a stranger
situation: You are stopping work before it is finished and something else will pick it up next.
revision: 1
scope: >
  Written for agent work that crosses a session boundary, a context reset, or a handoff to a
  different agent or person than the one who did the work — anywhere continuity of memory cannot be
  assumed. Less useful where the same continuous session or person will resume within minutes.
capabilities:
  - A place to write the note that the next reader will actually open before acting
  - Stable identifiers to point at — file paths, a branch name, a commit SHA, a work-item ID
enforcement:
  mechanism: >
    None automatic. This is a fixed field list applied by the writer at the moment of stopping, not
    a system that checks the note afterward.
  checks: Nothing checks it. The list exists to be applied by hand.
  not_checked: >
    Whether the fields are filled in accurately, whether the next reader opens the note before
    acting, and whether the facts in it — a path, a SHA, a branch — are still true by the time it is
    read.
evidence: observed-here
evidence_note: >
  Observed across handoffs where a note written informally required the next session to spend time
  reconstructing state that the note could have stated directly.
prior_art:
  - >
    Google's Site Reliability Engineering guidance on incident management calls for an incident
    timeline that includes what responders tried, including attempts that failed, not only the fix
    that worked.
adds: >
  The complete field list beyond the attempted-fix field, and the framing that the note is written
  for a stranger rather than for a continuous future self.
---

# The handoff is written for a stranger

**The obvious answer:** leave a status note before you stop — what you were doing, what's next.

**What it misses:** a note like that is written for "future me," and future me is assumed to still
have the context that is currently in your head. Across a session boundary, a context reset, or a
handoff to a different agent, that assumption is false. The note reads back as reasonable and is
missing exactly the details the next reader cannot reconstruct: which of three similar files, which
of two attempts already failed, which branch.

**The practice:** write the note as if it will be read by something that has never seen this session
— no shared memory of what "the fix" or "the usual place" refers to. That forces the specific
information onto the page instead of leaving it implicit.

## When to consult this

You are stopping before the work is finished — end of session, a context reset, or handing the
remainder to a different agent or person — and you're about to write a note about it.

## What happened

A handoff written informally ("continue the fix, should be close") reached a fresh context with no
memory of the session that wrote it. The next reader had to re-derive which files were touched,
which branch held the work, and what had already been tried, before doing anything else — work the
first session could have written down in less time than it took to re-derive.

## What the practice changes

The note carries a fixed set of fields, every time, regardless of how obvious the next step feels to
the person writing it:

- The goal, stated in one sentence a stranger can understand without the rest of the conversation
- What is actually done, not what was intended
- The next increment — the next concrete step, not the eventual outcome
- Exact identifiers: file paths, branch name, commit SHA — not "the usual file" or "that branch"
- What was already tried that failed, and why, so it isn't retried
- What is waiting on a human, named as such
- The exact command that verifies the current state
- Related work referenced by its ID, not by a description that can drift from the thing it describes

## When it helps

Any work that might not be finished by the session that started it — most work, once more than one
session or more than one agent is in play.

## What it costs, and when to skip it

Writing a note to this schema takes real time when the obvious next step really is obvious. And the
note is only accurate at the moment it's written: a SHA stops pointing at anything useful after a
rebase or a squash, a path moves, a branch gets deleted. The schema doesn't fix that — it just makes
the note specific enough to be checked against current state instead of vague enough to seem safe
regardless of what changed underneath it. Skip the full schema when the same session, with the same
live context, will pick the work back up within minutes; the note is solving a discontinuity that
hasn't happened yet.

## How you can tell whether it worked

The next reader acts on the note without sending a clarifying question back to whoever wrote it, and
without re-deriving something the note should have stated.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| A brief status line ("continuing the auth work") | Reads as informative to the person who wrote it and is missing everything a stranger needs |
| Rely on commit history | A commit message says what changed, not what's still open, what's waiting on a human, or what was already tried and failed |
| A verbal or chat handoff to one named person | Doesn't survive the work landing with a different person or agent than the one addressed |
| Write for "future me" | Assumes continuity of memory that a session boundary or context reset does not preserve |
| A ticket with a title and a link | Requires opening and cross-referencing other systems before the state is clear, when the note could have said it directly |

## Prior art, and what this adds

Google's Site Reliability Engineering guidance on incident management already calls for a timeline
that includes what was tried and failed, not only the fix that worked — that field is not new here:
"what was already tried that failed" is not something nobody writes down. What this adds is the rest
of the schema around it (exact identifiers, the next increment, what's waiting on a human, related
work by ID) and the framing: write for a stranger, not for a future self assumed to remember.

## Where this is most likely to be wrong

- **Workflows that rewrite history before pickup** — a rebase, a squash, or a force-push between the
  note being written and read invalidates the SHA the note pointed at, and the schema doesn't detect
  that on its own.
- **Fast handoffs within the same live session**, where the overhead of the full schema exceeds the
  cost of just continuing.
- **A single continuous human owner** who reliably holds context across the gap — the discontinuity
  this practice defends against may not exist in that setup.
