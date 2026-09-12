---
name: independence-is-a-property-of-the-seat
title: Independence is a property of the seat, not of the model
situation: You want a second opinion on work you just did, or on work one of your workers just did.
revision: 1
scope: >
  Applies wherever a reviewer is another instance of a language model. Assumes you can control what
  the reviewer is shown. Where you cannot start a genuinely fresh context, the practice is only
  partly implementable and the entry says so rather than pretending otherwise.
capabilities:
  - The ability to start a context that has not seen the authoring work
  - Control over exactly what material the reviewer receives
enforcement:
  mechanism: The reviewer is dispatched with named artifacts or a commit, never with the drafting transcript
  checks: That the reviewer's inputs are enumerable and were assembled deliberately
  not_checked: >
    Whether the review was competent, and whether the dispatching prompt smuggled the author's
    conclusion back in through its framing
evidence: reproduced-here
evidence_note: >
  Repeatedly observed in one environment: reviews that followed the drafting confirmed the drafter,
  and the same model in a clean context on the same artifact raised objections the first pass had not.
prior_art:
  - Blind peer review conceals author identity from the reviewer
  - Separation-of-duties requirements generally forbid the author from approving their own work
adds: The variable that matters is not who the reviewer is but what the reviewer was shown.
---

# Independence is a property of the seat, not of the model

**The obvious answer:** to get an independent review, use a different model, or a stronger one.

**What it misses:** a different model that watched the work being drafted is not independent. It saw
the reasoning, the false starts, and usually the author's own verdict, and it will tend to confirm
them. Meanwhile the *same* model, in a context that has never seen any of that, reviewing the
artifact alone, will find things the first pass did not.

Model identity is the variable that is easy to change, which is why it gets changed. Exposure is the
variable that matters.

**The practice:** define independence by what the reviewer was shown. A reviewing seat is
independent when it did not author the work, did not observe the authoring, and received the
artifact rather than the narrative.

## When to consult this

You are about to ask for a review of something you just produced. Or you are about to conclude that
review is covered because a second, better model looked at it.

## What happened

Reviews dispatched at the end of a session — same conversation, full drafting history in context —
came back agreeing. Not with flattery, with specifics: they engaged with the reasoning and endorsed
it. The reasoning was the drafter's reasoning, which they had watched being constructed.

The same artifact handed to a clean context, with no history and no proposed verdict, produced
substantive objections, including to premises the first reviewer had accepted without comment.

Two follow-on observations that changed how dispatches are written:

- **Posting the author's conclusion destroys independence even in a fresh context.** A clean
  reviewer told what the author decided reviews the decision instead of the work.
- **A relayed review can invert.** Passing a reviewer's findings through the author, summarized,
  reliably softens them. If independence matters, the review has to arrive intact.

## What the practice changes

You stop asking "which model should review this" first and start asking "what has this seat seen."
Concretely:

- Dispatch reviewers with the artifact — a file, a diff, a commit — not with the conversation.
- Do not include your own verdict, your confidence, or which parts you are worried about. The last
  one feels helpful and is the most damaging, because it defines the search area.
- If the review needs to be relayed, relay it verbatim.
- If your environment cannot produce a fresh context, say that the work is **unreviewed**, rather
  than counting a non-independent pass as a review. A wrong label is worse than a missing one.

## When it helps

Most when the work is foundational — a plan, a standing rule, an enforcement mechanism, anything
outward-facing — where a confirming review is actively worse than no review, because it converts an
unchecked decision into a decision that appears checked.

## What it costs, and when to skip it

A fresh context has to be told what it needs, which costs tokens and effort, and it will sometimes
raise objections a reviewer with the full history would have known were already settled. You will
spend time re-explaining. That is the price of the seat, and it is mostly the same property that
makes it useful.

Skip it for small, reversible, low-cost work. Not everything needs a seat.

## How you can tell whether it worked

Independent seats sometimes return findings that change the work. If every review from your
"independent" reviewer has confirmed the author, the seat is not independent — that is the
measurement, and it is available without any additional instrumentation.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| Use a stronger model as reviewer | Capability and independence are different axes; a stronger model that watched the drafting still confirms it |
| Use a different vendor's model | Better than nothing, but still fails if it saw the authoring — and it gets treated as sufficient, which is the harm |
| Ask the same context to "review critically" | Self-critique in the authoring context reliably produces findings the author already anticipated |
| Have the author summarize the work for the reviewer | The summary encodes the author's framing; this is the relay-inversion failure in a different order |
| Multiple reviewers in the same context | Correlated exposure; they agree with each other and with the author |

## Prior art, and what this adds

Blind peer review removes the author's *identity* from the reviewer's view, on the theory that
identity biases judgment. Separation-of-duties controls forbid self-approval. Both are about who the
reviewer is or is not.

The addition is that for model reviewers the binding constraint is exposure, not identity — a
reviewer who knows exactly whose work it is but has seen only the artifact is independent, and a
reviewer who has no idea whose work it is but watched it being written is not. That inverts which
knob to reach for.

## Where this is most likely to be wrong

- **Environments without a fresh-context primitive**, where this is unimplementable and the honest
  move is to label work unreviewed rather than approximate the seat.
- **Deeply contextual work**, where a clean reviewer may lack enough background to say anything
  useful and the re-explanation cost swamps the benefit. The boundary is not well established.
- The claim that the same model in a clean seat outperforms a different model in a contaminated one
  is drawn from one environment and one style of dispatch. It is the most falsifiable claim here and
  the most useful one to test.
