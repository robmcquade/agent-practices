---
name: review-depth-comes-from-cost-not-subject
title: Review depth comes from cost, not subject matter
situation: You are deciding how much scrutiny a piece of work needs before it lands.
revision: 1
scope: >
  Applies to sizing review effort for any change, human or agent-authored. Works best where any
  reviewer touching the work — not only the one who set the original level — can raise it in a way
  the author cannot override; without that, the asymmetric rule below has nothing to enforce it and
  is closer to a working preference than a mechanism.
capabilities:
  - A way to write the decision down before review starts, so it can be checked later
  - Any reviewer's ability to raise the level, unilaterally, in a way the author cannot override
enforcement:
  mechanism: >
    A short written statement of the specific failure being guarded against, recorded before the
    review level is chosen, plus a rule that any reviewer may raise the level and none may lower it
  checks: That the statement exists and names a failure, not a topic, and that a raised level stands
  not_checked: >
    Whether the stated failure is the real one, whether the chosen level is actually proportionate to
    it, whether someone picked a narrow failure statement on purpose to keep the level low, and
    whether a given raise was itself warranted — nothing screens a raise for being mistaken or
    automated before it becomes permanent
evidence: observed-here
evidence_note: >
  Converged in one environment after review effort tracked how alarming a change sounded rather than
  what a mistake in it would cost to undo, and ordinary-looking changes with real, hard-to-reverse
  cost repeatedly got the light review their subject matter implied.
prior_art:
  - >
    OWASP's Application Security Verification Standard defines graduated verification levels chosen by
    an application's risk profile, which is substantial overlap with sizing scrutiny to consequence
    rather than treating all checks the same
adds: >
  The asymmetric rule that anyone may raise the level and nobody may lower it, and requiring the
  specific guarded failure to be written down before the level is picked, rather than picking the
  level from the application's general risk profile
---

# Review depth comes from cost, not subject matter

**The obvious answer:** match the review to how the change sounds. Anything touching
authentication, payments, or infrastructure gets heavy review; a copy change, a default value, or a
data migration gets a quick look, because it sounds routine.

**What it misses:** subject matter is a proxy for cost, and it is a bad one. A change to a default
configuration value or a one-time data migration can be far more expensive to undo than a change to
code that merely sounds dangerous — the migration ran against real data before anyone reread it, and
the default is now baked into every record created since. Meanwhile a change that sounds alarming is
sometimes trivially reversible: feature-flagged, staging-only, or backed out with one command. The
words in the diff do not track the cost of getting it wrong.

**The practice:** before choosing a review level, write down the specific failure you are guarding
against — not "this touches auth" but "this could grant access it shouldn't, and that would be
hard to detect after the fact." Size the review to what recovering from *that* failure would cost,
not to how the change reads. Two clauses carry the weight: **anyone may raise the review level, and
nobody may lower it**, and **being an obvious, everyday-looking change does not demote something
that is expensive to recover from.**

## When to consult this

You are about to wave a change through because it "doesn't sound like the risky kind," or about to
schedule heavy review for something because it touches a component with a scary name. Also: someone
is proposing to reduce the review level on a change that already has one set.

## What happened

Review effort tracked the vocabulary of the change rather than its consequences. Changes with
security- or infrastructure-sounding names drew reviewers by default, whether or not a mistake in
them would have been easy to catch and undo. Changes that looked routine — a config default, a
migration, a value used by every downstream calculation — moved with light review, because nothing
about their surface suggested danger, and some of them were the hardest to walk back once applied.

## What the practice changes

- The review level is chosen from a written statement of the failure being guarded against, not from
  the topic of the change. If no one can state a specific failure, that itself is informative — it
  usually means the level is being picked from vocabulary.
- Raising the level is available to anyone who notices the stated failure understates the real cost.
  Lowering it is not available to anyone, including the author — a level once set only goes up.
- An everyday-looking change does not get a pass for looking everyday. The question is never "does
  this look dangerous," it is "what would it cost to recover from if this is wrong."

Once the level is set, [Set the review budget before review starts](review-that-terminates.md)
governs how many passes that level actually gets — this practice answers how deep, that one answers
how long.

## When it helps

Any environment where the volume of changes exceeds what uniform heavy review can absorb, so some
sizing decision has to be made — and especially where an agent is making that sizing call quickly,
with no time pressure to be dramatic about a scary-sounding name or dismissive about a boring one.

## What it costs, and when to skip it

Writing the failure statement down takes a deliberate moment before work starts, for every change
that isn't obviously trivial — overhead that a topic-based rule of thumb does not have.

It is also gameable: a narrow, carefully worded failure statement produces a low level for a change
that has a broader real failure mode nobody wrote down. The rule only constrains what happens after
a failure is named; naming the wrong one is not caught by this practice.

The asymmetry is the sharpest cost. Because a level can only go up, a level set too high by an
overcautious first read stays high for the life of that piece of work — there is no reset, fresh
review or otherwise, that lowers it back down within that work. A new piece of work starts its own
level from scratch, but that is a new assessment, not a correction to this one. This is a deliberate
tradeoff: the practice accepts some amount of permanent over-review in exchange for making it
impossible to quietly talk a level back down under pressure to ship.

The raise right being unbounded is a second, separate cost, not just a consequence of the first: any
single reviewer — including a mistaken one, or an automated one — can lock in a permanently elevated
level with one call, and nothing in this practice screens the raise itself for whether it was
warranted. That is accepted deliberately, as the price of removing the ability to talk a level back
down; it is not free, and nothing here bounds it beyond ordinary social pressure not to raise levels
carelessly.

Skip the rule where that tradeoff is backward — for genuinely trivial, reversible work, forcing a
written failure statement on everything is ceremony with nothing behind it.

## How you can tell whether it worked

Look at cases where the chosen level was later raised. If the record shows some, the raise-only rule
is doing something. If a review level has ever been quietly lowered after being set, the rule is not
being followed regardless of what is written about it.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| Classify by topic or keyword (security, payments, infrastructure) | A topic correlates with how alarming a change sounds, not with how expensive a mistake in it is to undo |
| Let the author choose the level | The author is the worst-placed judge of the cost of their own mistake, and has the most reason to pick low |
| One fixed review depth for everything | Wastes effort on changes that are cheap to fix, and still under-reviews the expensive ones that don't announce themselves |
| Size review by the amount of code changed | Diff size does not track cost of failure; a one-line default value can be more consequential than a large, easily reverted refactor |
| Escalate only when someone happens to object | Passive — it depends on a reviewer noticing before the level is set, rather than on the level being set correctly in the first place |

## Prior art, and what this adds

The Application Security Verification Standard defines multiple levels of verification rigor and
expects an application to be assigned a level based on its risk profile, rather than treating every
check the same. That is the same core idea: scrutiny should track consequence.

What this adds is narrower and more mechanical: the failure being guarded against is written down
per change, not assigned once from a general risk profile, and the level that results can only be
raised afterward — never renegotiated downward by the same process that set it.

## Where this is most likely to be wrong

- **Environments with a mandated compliance level** — regulated settings where the review tier is
  fixed by an external requirement regardless of the failure statement, and this practice's sizing
  logic has nothing to attach to.
- **Solo work with no one else positioned to raise the level.** The asymmetric rule depends on there
  being an "anyone" distinct from the author; without one, it collapses to the author's own judgment,
  which is the case the rule exists to guard against.
- **Under real time pressure, writing the failure statement is exactly the step that gets skipped**,
  and skipping it is invisible unless someone is specifically checking for it.
