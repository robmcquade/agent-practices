---
name: any-addition-requires-a-deletion
title: Any addition requires a deletion
situation: About to add a new rule, clause, or line to a standards document that already governs
  work in progress.
revision: 1
scope: >
  Written for a living rules document that people or agents are expected to read in full before
  acting — a definition of done, a style guide, a standing set of operating rules — not for an
  append-only record like a changelog or decision log, where growth is the point. The general claim
  that a document meant to be read before acting has to stay short to keep being read is close to
  objectively true; the specific one-for-one ratio is a policy choice, not a law.
capabilities:
  - Authority to remove a rule in the same change that adds one, without a separate and slower
    approval track for removals
enforcement:
  mechanism: A stated constraint, checked when a rule is proposed, that the document's net length
    or rule count does not grow
  checks: Whether an addition landed without a paired removal, or landed by cutting a rule that
    was still catching real failures instead of being rejected outright
  not_checked: >
    Whether the rule that was removed was the right one to cut, whether a genuinely load-bearing
    rule was cut only because it was easier to give up than to argue against the new addition, and
    whether the "deletion" was real or two rules were quietly compressed into one denser
    sentence — length held constant, readability not
evidence: observed-here
evidence_note: >
  Adopted as a stated constraint in the header of one environment's definition-of-done document.
prior_art:
  - The UK Government's "One-in, One-out" regulatory management policy, later tightened to
    "One-in, Two-out" and "One-in, Three-out," which requires a new regulation carrying a cost to
    business to be offset by removing existing regulatory cost before it can proceed
adds: Applying the same forcing mechanism to an internal engineering standards document, where the
  resource being protected is a reader's attention rather than a regulated business's compliance
  cost.
---

# Any addition requires a deletion

**The obvious answer:** append the new rule — if it's a good idea, add it, and the document is
just getting more complete.

**What it misses:** a rules document that people are expected to read before acting is not a
reference you search, it's something you're supposed to hold in your head. Past a certain length,
additions stop making it more complete and start making it less followed — a new rule buried among
a hundred others nobody rereads in full does not reliably get applied, and the older rules compete
for the same limited attention as the new ones.

**The practice:** adding a rule requires removing one, in the same change, at the same time.
Whoever wants to add a rule has to find something in the document to cut to make room for it. That
is a real cost, and it filters out additions that are not worth paying it. When nothing in the
document is safe to cut — every candidate rule is still catching real failures — the addition is
rejected outright. The ratio is never satisfied by removing a control to make room for something
new; a rule that is still doing its job does not become cuttable just because something else wants
the space.

## When to consult this

About to add a new line, clause, or rule to a document meant to be read in full before acting,
rather than searched when needed.

## What happened

A rules document accretes one line at a time, each individually justified by something that
actually went wrong. Nobody removes anything, because removal has no natural trigger the way
addition does — a new failure prompts a new rule, but nothing prompts a review of the old ones.
Eventually the document's length becomes its own risk: reading it in full before acting stops
being realistic, so people and agents start pattern-matching on the parts they remember, and rules
that are still technically in force but no longer read start silently failing to catch the thing
they were written for.

## What the practice changes

The trade happens at write time, by the person proposing the addition, in the same change — not as
a cleanup exercise scheduled for later that competes with everything else for priority and rarely
happens. The cost of adding is visible immediately, to the person paying it, instead of deferred to
someone else at an unspecified future date. If nothing in the document is safe to cut, what gets
rejected is the addition — never a control, removed just to balance the ledger.

## When it helps

Any document whose value depends on being read in full before acting rather than skimmed or
searched — an operating-rules file, a definition of done, a style guide meant to be internalized
rather than looked up.

## What it costs, and when to skip it

The stop condition narrows this risk but does not remove it: "safe to cut" is still a judgment call,
made under pressure to land the new rule, by the same person who wants the addition to go through.
A rule can be misjudged as no longer load-bearing and cut anyway, with no guarantee that was the
least valuable rule in the document — only that it was the one someone convinced themselves was
safe. Its absence sometimes isn't noticed until the situation it used to catch happens again. The
honest cost on the other side is real too: some additions that would have been worth making are
rejected outright because nothing was safe to cut, and the document does not get them. The practice
also has a specific failure mode: it rewards broad, vague rules over narrow,
specific ones, because a broad rule is cheaper to defend at cut time relative to how much it
covers, while a precise rule is an easy target when someone needs room for something else — so the
document can drift toward wording broad enough to need re-interpreting later, close to the problem
length was supposed to solve. And the constraint is on net length, not on real reduction — two
rules quietly compressed into one denser sentence satisfies it without making the document any
easier to hold in your head.

## How you can tell whether it worked

Length or rule count staying flat is not the test — that can hold while safety quietly degrades, by
cutting real rules or by compressing two into one denser sentence. The test is whether a mistake a
cut rule used to catch has recurred since it was removed: if one has, the ratio was paid for with the
thing the document exists to prevent, and that counts as the practice failing regardless of what the
line count shows. A second, cheaper signal: there is a visible record of additions that were
rejected outright because nothing was safe to cut. If that count is always zero, the stop condition
is not being enforced — only the ratio is.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| Periodic pruning review (a scheduled cleanup pass) | Competes with everything else for priority and reliably keeps not happening; the document only shrinks if someone remembers to schedule the meeting |
| No forced trims — let it grow, rely on headers and search to navigate it | Fine for a reference you look things up in; this is a document meant to be read in full before acting, where length stops being free |
| Require written justification for the addition instead of a deletion | Justification is cheap to produce for almost any proposed rule and does not cost the proposer anything comparable to actually giving something up |
| A hard length ceiling with no addition tied to a specific removal | Pressure to cut only appears once the ceiling is hit, so additions accumulate freely up to the wall and then force a scramble |
| Trust editors to keep it tight, no formal constraint | This is the status quo that produced the accretion in the first place |

## Prior art, and what this adds

The UK Government's "One-in, One-out" policy required a new regulation carrying a cost to business
to be offset by removing existing regulatory cost before it could proceed, and later versions —
"One-in, Two-out," then "One-in, Three-out" — required removing more than one rule's worth of cost
per addition. Its target was net cost to business, not a page count or rule count, and its stated
purpose was to make departments hesitate before adding a new regulatory burden rather than to make
any single document shorter.

What this adds is the same forcing function applied to an internal engineering standards document
instead of statutory regulation, where the unit being conserved is a reader's attention and working
memory rather than measured compliance cost — a count or length proxy standing in for a
harder-to-measure real cost, which is weaker than the original mechanism but far cheaper to apply.

## Where this is most likely to be wrong

- **Reference documents that are searched rather than read start to finish** — a comprehensive
  index or FAQ — where length isn't the enemy, findability is, and a forced deletion could remove
  genuinely needed material with no attention-budget benefit.
- **Early-stage documents that are still legitimately incomplete**, where forcing a trade before
  there is enough real content to responsibly cut from encourages deleting something load-bearing
  just to satisfy the mechanism.
- **Documents with multiple independent owners contributing without coordination**, where "my
  addition, your deletion" can turn into a dispute over whose rule gets cut, or a race to add a
  rule before someone else's addition forces a trade nobody agreed to.
