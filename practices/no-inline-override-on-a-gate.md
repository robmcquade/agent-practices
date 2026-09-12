---
name: no-inline-override-on-a-gate
title: A gate with an inline override is not a gate
situation: You are building a check that can block progress — a merge gate, a pre-push hook, a deploy check, anything with the authority to say no.
revision: 1
scope: >
  Applies to automated gates enforced in a shared or fast-moving environment, where the same person
  who would use the gate could also be the one under pressure to get past it. Assumes a genuine
  escape hatch is sometimes needed and that it can be built as a separate path rather than a flag on
  the gate itself.
capabilities:
  - The ability to make the gate hard-fail with no bypass flag of its own
  - A separate path for a genuine override that is reachable without being adjacent to the gate
enforcement:
  mechanism: The gate's own interface accepts no flag, environment variable, or argument that skips its check
  checks: That refusing the gate on the command line, with no other channel, produces no way through
  not_checked: >
    Whether people work around the gate entirely outside its interface — editing it, disabling it,
    or removing it from the path that runs it — and whether the separate escape hatch, once reached,
    is used with the deliberation it is meant to require
evidence: observed-here
evidence_note: >
  In one environment running several checks that protect shared, hard-to-recover state, the gates
  refuse outright with no bypass flag; the override, where one exists, is a separate, explicitly
  acknowledged step rather than an argument next to the failing command.
prior_art:
  - >
    GitHub's bypass-request workflow for repository rulesets is a published instance of a separate,
    approved escape hatch: authorized reviewers are notified and must approve or deny before the
    push proceeds, rather than the pusher supplying a flag that skips the rule directly
  - >
    Git's own documentation for its force-push flag warns that it disables safety checks and can
    discard commits, which is a warning about the same flag it ships
  - >
    Mistake-proofing and guardrail design describe the general principle of making the wrong action
    hard to take by accident, which this is one application of
adds: >
  The general rule that a gate's own interface should expose no override, and the specific reasoning
  for it: an override sitting next to the error is reached for under time pressure, which is the same
  condition the gate exists to catch
---

# A gate with an inline override is not a gate

**The obvious answer:** the check fails loudly, prints a clear warning, and ships with a flag —
`--force`, `--skip-checks`, an environment variable — for the times you really need to get past it.

**What it misses:** the flag sits right next to the failing message, in the same command, available
to the same person, at the exact moment they are already under pressure. That moment — something is
broken, a deadline is close, the fastest path is one flag away — is precisely the moment the gate was
built for. An override that costs nothing more than typing one extra word gets used the first time it
is convenient, not only the time it is warranted, and a gate that can be silenced by its own caller is
not a gate. It is a warning with a snooze button.

**The practice:** the gate refuses outright on an unmet precondition and provides **no inline
override** — no flag, no environment variable, nothing in its own invocation that skips the check.
An escape hatch can still exist, but it is a separate, deliberate act: a different command, a
different approval step, something that requires stating plainly what is being bypassed and why —
not an argument sitting beside the error text.

Two clauses that go with it:

- **The gate refuses; it does not force its way past the problem itself.** It stops and reports,
  and leaves the decision to whatever handles the separate override — it does not attempt to repair
  or route around the unmet precondition on its own initiative.
- **The gate re-checks its own preconditions rather than trusting a caller's word that they were
  met.** A gate that accepts an assertion instead of verifying it has the same defect as one with an
  inline flag — it can be told to proceed by exactly the person under pressure to proceed.

## When to consult this

You are writing anything with the authority to block — a merge gate, a pre-push hook, a release
check, a guard around a shared resource. Also: you are about to add a flag to an existing gate "for
emergencies," or a caller is asking for one.

## What happened

The general failure this guards against is well understood without needing a specific incident to
illustrate it: a gate is given a bypass flag for legitimate emergencies, and the flag gets reached
for under the same time pressure that makes people careless, not only under the emergencies it was
scoped for. Because the flag is right there, in the same command, it lowers the cost of skipping the
gate to roughly zero — one extra argument — and a check that can be skipped for free is a suggestion,
not a gate.

## What the practice changes

- **Refuse, don't force.** On an unmet precondition, the gate stops and reports what failed. It does
  not attempt a best-effort workaround on the caller's behalf.
- **No flag in the gate's own interface.** If an override is going to exist, it does not live inside
  the command that just failed.
- **The override is a separate, deliberate act.** A different command, a distinct approval step, or a
  second party — something with enough friction that using it means consciously choosing to, not
  reflexively appending a word to a command that just refused.
- **The gate verifies its own preconditions.** It does not accept a caller's claim that a
  precondition was already satisfied elsewhere; it checks, every time it runs.

## When it helps

Gates that exist specifically to protect against decisions made under pressure — a merge into shared
history, a deploy, an irreversible or hard-to-undo operation. The more likely it is that the person
invoking the gate is in a hurry, the more the adjacency of an override matters.

## What it costs, and when to skip it

This is a real cost, not a theoretical one: refusing with no inline override means someone can get
genuinely stranded in a real emergency if the separate escape hatch is slow, undiscoverable, or
unreachable at the moment it is needed. The practice only works if that separate path actually
exists and actually works when called on — a gate with no reachable override at all is not a safer
gate, it is an outage waiting for the day something legitimately needs to get through. Building the
practice without building and testing the escape hatch is worse than not building the practice.

Skip the ceremony for gates guarding cheap, reversible mistakes, where the cost of an occasional bad
override is lower than the cost of maintaining a separate approval path for it.

## How you can tell whether it worked

The gate has actually stopped someone at least once, and that stop is visible in a record somewhere
rather than silently worked around. When the separate escape hatch is used, it leaves a trace of
deliberate acknowledgment — not just a flag buried in a command someone ran and moved past.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| `--force` flag with a confirmation prompt | The prompt is dismissed the same way the flag would have been, by rote, under the same pressure that motivated typing it |
| Loud warning, no hard block | An advisory-only check is ignored precisely in the situation it exists to catch — that is what "under pressure" means |
| Inline override that requires typing a reason string | The reason still lives in the same command, typed alone, with no second party and no real friction beyond a few extra words |
| A narrower flag that skips only some of the gate's checks | Shrinks what gets bypassed but keeps the override adjacent to the failure — adjacency, not scope, is the defect |
| Time-delay before an inline override takes effect | Can be started early and left running, and still requires no acknowledgment of what is being bypassed |
| No override of any kind, ever | Turns a genuine emergency into a dead end with no path forward; rejected because the gate then causes the outage it was meant to prevent |

## Prior art, and what this adds

GitHub's bypass-request workflow for repository rulesets is a published instance of the separate-path
idea: authorized reviewers are notified and must approve or deny before a blocked push proceeds,
rather than the person being blocked supplying a flag that clears the rule themselves. Git's
documentation for its own force-push flag is explicit that the flag disables safety checks and can
lose commits — a warning attached to the very flag it ships, which is a smaller version of the same
tension. Mistake-proofing and guardrail design more broadly describe making the unwanted action hard
to reach by accident, which this is one specific application of.

What this adds is the general rule stated independent of any one gate — no inline override, ever,
regardless of what the gate protects — and the specific reason: adjacency to the error is what turns
an override into the default path, because it is used under the exact time pressure the gate exists
to interrupt. No published source examined states that reasoning directly.

## Where this is most likely to be wrong

- **Solo settings with no second party available.** A separate escape hatch that still resolves to
  the same one person, just through a different command, keeps the deliberate-act framing but loses
  the independent-approval benefit entirely.
- **Emergencies with no reachable approver.** If the separate path depends on someone else being
  available and they are not, the gate has produced a genuine outage rather than a safer default.
- **Low-stakes gates**, where building and maintaining a separate override path costs more than the
  rare bad override it prevents — the practice is meant for gates protecting something expensive to
  recover from, not every check that can fail.
