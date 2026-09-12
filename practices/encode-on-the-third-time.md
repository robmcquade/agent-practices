---
name: encode-on-the-third-time
title: Encode on the third time
situation: You notice you are making the same judgment call for at least the third time.
revision: 1
scope: >
  Written for judgment calls that recur across sessions, tasks, or multiple agents that don't share
  memory implicitly — where nothing currently records that the call was already made and settled.
  Partly working style: how eagerly to encode is a preference, not an objectively correct constant.
capabilities:
  - A checklist or standards document that is actually consulted before the call is made again
  - A durable memory or notes mechanism that persists across sessions
  - The ability to add an automated check or hook, for the calls that support one
enforcement:
  mechanism: >
    None automatic. Nothing detects the third occurrence; it's noticed by whoever is making the call
    and acted on by hand, in the same session.
  checks: Nothing checks that encoding happened, or that it happened at the right rung of the ladder.
  not_checked: >
    Whether the call was actually the same one three times or three similar-looking ones, and
    whether the encoded rule still fits once the situation that produced it changes.
evidence: observed-here
evidence_note: >
  Observed where the same decision point was independently re-reasoned more than once, sometimes
  reaching different answers, because nothing had recorded that it was already settled.
prior_art:
  - >
    The Rule of Three, as published in Martin Fowler's "Refactoring" — duplication is tolerated
    twice and extracted on the third occurrence.
adds: >
  Applying that trigger to repeated judgment calls rather than repeated code, and a
  cheapest-mechanism-first ladder for what "extracted" means when there's no function to pull out.
---

# Encode on the third time

**The obvious answer:** remember it, or jot down a quick note for yourself.

**What it misses:** memory doesn't cross a session boundary, and a personal note doesn't reach a
different agent facing the identical decision. Making the same call three times isn't a sign of a
hard problem — after the third time, it isn't a judgment call anymore, it's a rule nobody wrote down.

**The practice:** on the third occurrence, in the same session it happens in, encode it in the
cheapest mechanism that will actually hold it: a checklist line first, a durable memory entry if
that's not enough, an automated check or hook only once the cheaper rungs have demonstrably failed —
meaning the fourth occurrence happened despite the checklist line existing, or despite the memory
entry existing, in a place that should have been read.

## When to consult this

You notice, mid-task, that this is at least the second time you've reasoned through this exact
decision before — meaning the current one is the third.

## What happened

The same decision point (a formatting choice, a routing choice, a small policy call) got
independently re-derived on separate occasions, sometimes reaching different answers, because
nothing recorded that it had already been settled the first two times.

## What the practice changes

- The trigger is the third occurrence, not the first. Most first occurrences are genuinely one-off;
  encoding on the first produces rules for things that never recur.
- The mechanism is chosen cheapest-first: a checklist line before a memory entry before an automated
  check or hook. Escalating past a rung requires that rung to have demonstrably failed — the rule
  existed, in a place that should have been read, and the call was made wrong anyway.
- It has to happen in the same session as the third occurrence. Deferred, it's lost: the fourth
  occurrence becomes a fresh judgment call again because nobody remembers there was going to be a
  rule.

## When it helps

Decision points that recur across sessions or across multiple agents that don't otherwise share
memory — anywhere the same question gets asked again by something that wasn't there for the first
two answers.

## What it costs, and when to skip it

Encode too eagerly and the checklist becomes a thicket nobody actually reads before the tenth item,
which defeats the mechanism it's supposed to feed. It also risks freezing a call that was genuinely
context-dependent into a rule applied rigidly after only two real data points. See
[Any addition requires a deletion](any-addition-requires-a-deletion.md) as the counterweight: this
practice adds rules, that one is what keeps the pile from growing without bound.

## How you can tell whether it worked

The fourth occurrence is a lookup against the checklist or memory entry, not a re-derivation from
scratch — and it visibly takes less effort than the third one did.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| Keep relying on memory and context | Doesn't survive a session boundary or a different agent making the same call independently |
| Encode on the first occurrence | Most first occurrences are one-off; this produces rules for things that never happen again |
| Jump straight to an automated check or hook | Most judgment calls have no cheap automatable check; building one costs more than the calls it saves, or forces a nuanced call into a rigid gate |
| Wait for a reviewer to notice the pattern | Review is intermittent, and a reviewer doesn't always see the same repeated call the author sees |
| Never encode — treat every occurrence as fresh judgment | This is the failure mode being addressed, not an alternative to it |

## Prior art, and what this adds

The Rule of Three, as published in Martin Fowler's "Refactoring," is a well-known trigger for
extracting duplicated code on its third occurrence. It's about code duplication specifically, and it
supplies the trigger, not a ladder of mechanisms. What this adds is applying the same trigger to
repeated judgment calls rather than repeated code, and the cheapest-mechanism-first escalation order
for what "extracted" means when there's no function to pull the duplication into.

## Where this is most likely to be wrong

- **Fast-moving areas**, where the rule encoded on the third occurrence would need to change again
  soon anyway — encoding locks in something that's still in flux.
- **Single-session work with no third occurrence across a boundary** — the practice is solving a
  multi-session or multi-agent problem, and has nothing to do inside one session.
- **When the first two occurrences didn't actually reach the same answer** — encoding on the third
  would freeze a disagreement rather than a converged rule, and the disagreement is worth surfacing
  first.
