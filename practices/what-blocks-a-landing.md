---
name: what-blocks-a-landing
title: A finding needs a reachable case
situation: A review raises something and it is not clear whether it should block the merge.
revision: 1
scope: >
  Written for review of a change under active development — a diff, a pull request, a piece of
  work someone is trying to land. Assumes findings can be filed with a disposition other than
  blocking, rather than only accepted or rejected outright. The three-part test itself is close to
  objectively right for this setting; exactly where the pre-existing-defect line falls is partly a
  scoping choice one environment made.
capabilities:
  - A way to record a finding with a disposition other than "blocking" — tracked, won't-fix, filed
    for later
  - Someone who can distinguish what this change introduces from what the codebase already
    contained before it
enforcement:
  mechanism: A written three-part test applied to a finding before it is allowed to block
  checks: Whether a finding names a reachable case, the requirement it violates, and evidence
    connecting the two to the work under review
  not_checked: >
    Whether the evidence offered is actually correct, and whether a reviewer applies the test
    honestly rather than reaching for whichever disposition suits the outcome they already want
evidence: observed-here
evidence_note: >
  Converged after review rounds stalled on findings that were true observations about the code but
  were never connected back to the change under review.
prior_art:
  - Google's code review guidance marks certain comments as non-blocking by convention (a "nit,"
    left to the author's discretion), without stating a general test for where the blocking line
    falls
adds: A general test for where the blocking line falls in the first place — a three-part
  conjunctive check usable in ordinary review, not only formally tracked review — plus an explicit
  rule for when a pre-existing defect earns its way into blocking a change.
---

# A finding needs a reachable case

**The obvious answer:** whatever a reviewer raises blocks the merge — if it was worth writing
down, it's worth fixing before the change lands.

**What it misses:** a reviewer that can generate findings quickly will surface things that are
true about the code without being about this change: a pre-existing rough edge, an input nothing
in the call chain produces, a style preference with no requirement behind it. Being true is not
the same as being relevant to whether this change should land, and treating every true finding as
a blocker rewards volume over relevance.

**The practice:** a finding blocks only if all three hold — a reachable case exists (a path or
input that actually gets there, not a hypothetical one), the reviewer names the specific
requirement or behavior it violates, and there is evidence in the work under review connecting the
case to the violation. A supported trace is enough; reproducing it is not required. Missing any
one of the three, the finding is filed with a disposition instead of blocking.

A pre-existing defect — something already wrong before this change touched it — blocks only if the
change opens a path to it, makes it worse, depends on it to pass, or would cause harm during the
landing or verification itself. Otherwise it is filed rather than folded into this change's
disposition. A genuinely urgent hazard still gets contained on its own; filing it is not ignoring
it.

## When to consult this

A finding is on the table mid-review and it is not obvious whether it should block.

## What happened

As review scaled up, rounds started stalling on findings that were correct statements about the
code with no path back into the change under review: behavior the code already had before the
diff, inputs nothing in the call chain produces, or a preference with no requirement attached.

## What the practice changes

Distinguishing "true" from "blocking" needed to be explicit, because both reviewers and authors
trying to satisfy them will treat "true" as sufficient by default. A finding has to clear a stated
bar to block, instead of blocking by default and needing to be argued down afterward. The reviewer states the case, the requirement, and the evidence at the time
they raise it. This also forces a decision, at the moment a defect surfaces, about whether it
belongs to this change or to the state the change happened to land into. Filing a finding rather
than blocking on it is also what keeps a hard-to-resolve finding from silently consuming a review
pass — see [Set the review budget before review starts](review-that-terminates.md).

## When it helps

When review is cheap to start and produces a high volume of findings that are individually true
but only loosely connected to the change — the volume itself starts blocking landings, independent
of how serious any one finding is.

## What it costs, and when to skip it

Some defects that are hypothetical today become reachable tomorrow, and filing rather than fixing
them means a few get reached before anyone returns to the backlog. The test also costs more per
finding than flagging and moving on — the reviewer has to name a requirement and trace evidence to
it, not just point — even though it saves more than that in aggregate by cutting the number of
findings that reach blocking status. Skip the pre-existing-defect exception specifically when a
defect's downside is severe enough that filing it and moving on is not acceptable even
temporarily — that is exactly what the harm-during-landing clause and separate containment exist
to catch; the exception is not a license to leave a known hazard alone.

## How you can tell whether it worked

The share of raised findings that actually block drops without a matching rise in defects that
reach later stages. There is a visible backlog of filed, non-blocking findings with real
dispositions attached — not silence, and not a blocking pile.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| Any raised finding blocks | Rewards a reviewer's thoroughness in proportion to volume, not to relevance to the change |
| Block on severity ("major" findings only) | The same finding reads as major to whoever wants to block and minor to whoever wants to land; severity is a rating, and this needed a question with an answer |
| Trust reviewer discretion, case by case | The same reviewer reaches different verdicts on the identical finding depending on the day, with no traceable reason either time |
| Treat every pre-existing defect as in scope once noticed | Every review balloons into an audit of the whole codebase instead of the change in front of it |
| Treat every pre-existing defect as always out of scope | A change that leans on, worsens, or is unsafe to land next to a known defect ships anyway |

## Prior art, and what this adds

Google's code review conventions mark a class of comment as non-blocking by label, leaving it to
the author whether to act on it — useful, but it does not say what makes something blocking in the
first place, only that some comments are declared not to be.

What this adds to that convention is a general test for where the blocking line falls: a three-part
conjunctive check — reachable case, stated requirement, connecting evidence — cheap enough to apply
to every finding in ordinary review, not only formally tracked ones — plus an explicit rule for when
a defect that predates the change earns its way into blocking it, rather than leaving that judgment
implicit.

## Where this is most likely to be wrong

- **Regulated or safety-critical work**, where a known hazard may need mandatory disclosure or
  remediation regardless of whether the current change touches it, and where "filed, not blocking"
  could be misread as license to leave it alone.
- **Systems where "reachable" is expensive to determine** — deep call graphs, distributed
  systems — where a claim of "not reachable" is cheap to assert and expensive to verify, and the
  test can be gamed by asserting it rather than checking it.
- **Slow, human-paced review**, where the volume problem this addresses may not exist, and the
  added step of stating a requirement and tracing evidence for every finding is pure overhead with
  nothing to offset it.
