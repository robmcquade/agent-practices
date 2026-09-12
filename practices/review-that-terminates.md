---
name: review-that-terminates
title: Set the review budget before review starts, and let hitting it force a choice
situation: You are reviewing work, or having work reviewed, and there is no defined point at which review is finished.
revision: 1
scope: >
  Written for review that is performed by agents, or by agents and a human together, where a review
  pass is cheap to start and therefore easy to start again. Assumes someone has the authority to
  decide. In a team with an established approval process, the named decider is usually already
  defined and the budget is the part that is missing.
capabilities:
  - A way to run more than one review pass, and to count them
  - Someone or something with the authority to decide, distinct from the author
enforcement:
  mechanism: A written budget recorded with the work before the first review pass
  checks: That a pass count exists and that exceeding it is visible
  not_checked: >
    Whether the reviews were any good, whether the decider actually exercised judgment, and whether a
    pass was quietly split into two to stay under the count
evidence: observed-here
evidence_note: >
  Converged over repeated review cycles in one environment after unbounded rounds repeatedly
  consumed more effort than the work under review.
prior_art:
  - Required-approval counts on a protected branch bound the number of approvers, not the number of rounds
  - Escalate-rather-than-stall norms name a decider but generally do not fix a budget in advance
adds: The budget is fixed before review begins, and mutual agreement does not extend it.
---

# Set the review budget before review starts

**The obvious answer:** review until the work is good.

**What it misses:** "good" is not a stopping condition, it is a direction. A reviewer who still has
something to say always has something to say, and the marginal finding gets smaller while the cost
of the round does not. Review that can always justify one more round will take one more round.

**The practice:** before the first pass, write down the number of passes and who decides at the end.
Two passes for one-reviewer work, three when review is deliberately adversarial. Hitting the budget
forces a choice between three named outcomes: a different approach with a fresh count, a smaller
change, or park it.

## When to consult this

You are about to start a review cycle, or you are in round three and cannot say what would end it.
Also: you are the author, and each round is producing smaller findings than the last.

## What happened

Review rounds did not converge. Each pass produced findings, each fix produced a new surface, and
the total effort spent reviewing exceeded the effort of the original work by a wide margin on
several occasions. The findings were not wrong. They were real, and progressively less important,
and there was no point at which anyone was supposed to stop.

The specific failure that made the rule necessary: two reviewers agreeing with each other was being
read as a signal that the work needed another look, rather than as a signal that it was done.

## What the practice changes

Three things, and the third is the one that carries the weight:

1. The budget is written **before** the first pass, when nobody knows yet whether they will want
   more rounds. Deciding the budget after round two is deciding it under the influence of round two.
2. An authorized decider is named up front, so the end of the budget resolves rather than stalls.
3. **Agreement does not extend the budget, and neither does a reviewer still having something to
   say.** Without this, the rule is decorative — every round that wants to be round four can
   produce a justification for being round four.

Two other clauses that turned out to be load-bearing:

- **Parking is never "done."** If the budget is exhausted and the work is not ready, it parks with
  its state recorded. Calling that finished is how the rule gets used to launder unfinished work.
- **Hitting the budget never authorizes landing.** The budget bounds review effort. It does not
  convert an unresolved objection into an approval.

## When it helps

When review is cheap to start, which is exactly the condition agents create. A human reviewer's
calendar imposes a natural budget. An agent reviewer does not, so the bound has to be explicit.

## What it costs, and when to skip it

It costs you the genuinely valuable finding that would have come in round four. That is a real cost
and it is paid sometimes. The bet is that the expected value of round four is lower than its price,
and that bet is wrong occasionally.

Skip it when the cost of the failure is unbounded or irreversible — see
[Review depth comes from cost, not subject](review-depth-comes-from-cost-not-subject.md). A budget
is a tool for making review terminate, not a tool for making it cheap.

## How you can tell whether it worked

Total review effort per unit of work stops growing, and there are cases on record where the budget
was hit and the work was parked or re-approached rather than waved through. If the budget has never
once been hit, it is set too high and is not doing anything.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| Review until two reviewers agree | Agreement is cheap between reviewers sharing a rubric, and it correlates with shared blind spots rather than with correctness |
| A time box rather than a pass count | Time is not the scarce resource when reviews are fast; passes are, because each one triggers a fix cycle |
| Severity threshold — stop when only minor findings remain | Severity is assessed by the person who wants to continue; it slides |
| Let the author declare it finished | Removes the reviewer's only leverage, and the author is the worst-placed judge |
| No budget, rely on judgment | This is the failure mode, not the alternative |

## Prior art, and what this adds

Branch protection rules bound how many approvals a change needs, which bounds reviewers, not rounds
— a two-approval rule is entirely compatible with nine rounds of revision. Engineering cultures that
tell reviewers to escalate rather than block indefinitely address the same stall with a different
lever: they name a decider without fixing a count.

What is added here is the ordering and one exclusion: the count is fixed before anyone has an
opinion about the work, and consensus between reviewers is explicitly not grounds to extend it.

## Where this is most likely to be wrong

- **Regulated or safety-critical work**, where "we ran out of budget" is not an acceptable
  disposition and the practice could do real harm if applied literally.
- **Large teams**, where a fixed pass count may collide with an existing approvals process and
  produce two competing definitions of done.
- **Human reviewers**, whose availability already bounds rounds. The practice may be solving a
  problem you do not have.
- The exclusion on mutual agreement is the clause most likely to be a local artifact of one person's
  working style rather than a general truth. If you have evidence that reviewer consensus is a good
  signal in your environment, that is worth sending.
