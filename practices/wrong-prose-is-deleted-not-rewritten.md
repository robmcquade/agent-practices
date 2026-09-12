---
name: wrong-prose-is-deleted-not-rewritten
title: Wrong prose is deleted, not rewritten
situation: Review finds a comment, doc paragraph, commit message, or similar explanatory text that
  is wrong, stale, or overbroad.
revision: 1
scope: >
  Written for explanatory text whose whole effect is a human reading it — comments, commit
  messages, review and work-item bodies, internal documentation. Excludes operational instructions,
  documented contracts, and machine-parsed strings, which are behavior and follow ordinary review
  no matter how they're phrased. Outward-facing copy meant for an external reader keeps its own
  review level; the speed this trades for does not carry over to it. The prose/behavior boundary is
  meant as a general line; "a prose defect never blocks and never buys a round" is a policy choice
  one environment made about how much that boundary should cost.
capabilities:
  - Authority for the author to delete text without a second approval round
  - A place other than prose where evidence and rationale are expected to live — a review record,
    commit history, a tracked item — so deleting a sentence does not destroy the only copy of real
    information
enforcement:
  mechanism: The author deletes wrong prose on sight, before review is asked to weigh in; a
    reviewer who finds wrong prose treats deletion as sufficient closure
  checks: Whether a wrong claim in prose survived a review round as a rewritten sentence rather
    than a deleted one
  not_checked: >
    Whether the limitation the prose was pointing at still needs to be tracked somewhere else —
    that stays a judgment call, which is what keeps this from being a mindless string-deletion rule
evidence: observed-here
evidence_note: >
  Converged after review rounds spent repeated passes negotiating replacement wording for a
  comment or doc paragraph that was simply incorrect, rather than settling it once by removing it.
prior_art:
  - Google's documentation style guide advises deleting what you're certain is wrong and leaving
    alone what's merely unclear, while still permitting a submission to be held up if it leaves the
    documentation worse off
adds: Ruling prose defects out of the blocking calculus entirely — deletion is not just the
  preferred fix, it is the whole disposition, and an argument that the result reads "worse" cannot
  reopen it — plus a stated line between prose and behavior.
---

# Wrong prose is deleted, not rewritten

**The obvious answer:** fix the incorrect comment or paragraph — correct the wrong sentence in
place.

**What it misses:** a rewrite is a new claim, and a new claim can also be wrong. Fixing a comment
invites exactly the scrutiny that found the original problem: is the correction accurate, is the
phrasing defensible now, does it need its own justification. The instinct to improve the sentence
is what invites the next round of review, not what ends this one.

**The practice:** delete the wrong prose and put nothing in its place. The author deletes on
sight, before review — no softening, no hedge, no replacement sentence, no paragraph explaining
what used to be there. Deletion cannot introduce a new false claim, and it always ends the
finding.

Text a machine parses, an operational instruction someone follows, or a documented contract is
behavior, not prose — it goes through ordinary review like any other change; it does not get the
free deletion path. And never delete only a limitation while leaving the claim it limits
standing — if the caveat was the correct part and the claim underneath it is what's wrong, the fix
is to remove the claim, not to quietly drop the one sentence that qualified it.

## When to consult this

Someone — the author or a reviewer — notices that a comment, doc line, commit message, or similar
surface is wrong, stale, or broader than the code supports.

## What happened

Wrong prose kept generating its own review rounds. A reviewer flags a wrong comment, the author
proposes a correction, and the correction is itself a new claim — about precision, tone, or
scope — that draws the same kind of scrutiny the original sentence needed. The disagreement in the
second round was routinely about the replacement's wording rather than about the underlying
behavior, and it cost more review effort than the defect it was fixing.

## What the practice changes

A wrong-prose finding stops being negotiable. Removing the sentence satisfies the requirement
completely, because there is no longer a claim left for anyone to be right or wrong about. A stale
comment rarely has a reachable case tied to it, so
[A finding needs a reachable case](what-blocks-a-landing.md) already keeps most wrong comments
from blocking on their own terms. This practice covers what that one doesn't: even where a
reviewer would otherwise insist on a fix, deletion is accepted as that fix, and closing it that way
spends no round out of [the review budget](review-that-terminates.md).

## When it helps

Anywhere explanatory text gets produced quickly and cheaply — several agents writing comments,
commit messages, and docs at agent speed — where a wrong claim is cheap to introduce and a
negotiated correction has been expensive to review.

## What it costs, and when to skip it

Real information sometimes disappears along with the wrong sentence — a rationale that was mostly
right, a caveat with a true core — and because the pressure that would have prompted a careful
rewrite is gone, nobody comes back to re-add it. This is an ongoing cost, not a one-time one. Skip
the reflexive version of this when the text is the only surviving record of a decision's rationale
and there is no work-tracking system, design doc, or commit history that already holds it —
deleting there is a genuine loss, not tidying.

## How you can tell whether it worked

Wrong-prose findings close in a single round — deletion — instead of spawning a second round
arguing about replacement wording. The count of comments or doc lines that have been rewritten
more than once for the same underlying complaint trends to zero.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| Correct the sentence in place | The correction is a new claim and draws the same scrutiny as the one it replaced, often consuming as much review effort as the original defect |
| Flag it and let review negotiate a fix | Negotiation is the mechanism producing extra rounds; the disagreement is usually about tone or precision, not the underlying fact |
| Mark it TODO or FIXME and move on | The known-wrong claim stays live and readable in the meantime, and the marker is itself prose that goes stale the same way |
| Add a correction or caveat below the wrong line | Doubles the prose surface and leaves the original wrong claim in place for a reader who only sees the first sentence |
| Rely on reviewer judgment about when a rewrite is warranted | This is the status quo that produced repeated rounds over replacement wording |

## Prior art, and what this adds

Google's documentation style guide advises reviewers to delete what they are certain is wrong and
leave alone what is merely unclear, rather than trying to fix it during review. The same guidance
still permits holding up a submission that leaves the documentation worse off, which keeps open the
question of whether a deletion was good enough.

What this adds is closing that question: a prose defect, once deleted, never blocks and never buys
another review round regardless of whether the resulting text reads as "worse" by some other
measure — plus a stated boundary between prose, which this applies to, and behavior — operational
instructions, contracts, machine-parsed strings — which does not get the same free pass.

## Where this is most likely to be wrong

- **Outward-facing documentation for an external reader**, where a deleted explanation can leave a
  real gap the reader needed — "worse documentation" has a cost of its own there, separate from
  review-round cost, and blind deletion could ship a hole instead of a fix.
- **Text that is the only record of a decision's rationale**, with no work-tracking system, design
  doc, or commit history holding the same information elsewhere — deletion there is a genuine
  loss, not a shortcut.
- **Teams where deletion still routes through the same review process a rewrite would** — if
  removing a sentence needs the same approval a correction would, the practice saves nothing; it
  depends on the author having standing authority to delete on sight.
