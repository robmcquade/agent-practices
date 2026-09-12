---
name: no-inline-override-on-a-gate
title: A gate with an inline override is not a gate
situation: You are building a check that can block progress — a merge gate, a pre-push hook, a deploy check, anything with the authority to say no.
revision: 1
scope: >
  Applies to automated gates enforced in a shared or fast-moving environment, where the same
  principal who would use the gate could also be the one under pressure to get past it. Assumes a
  genuine escape hatch is sometimes needed and that it can be built as a path requiring different
  authority, rather than a flag on the gate itself. Excludes gates meant to fail closed with no
  caller-accessible override at all — security, integrity, compliance, and destructive-operation
  gates — where that assumption does not hold.
capabilities:
  - The ability to make the gate hard-fail with no bypass flag of its own
  - A separate path for a genuine override that requires different authority than the gate's
    caller holds — a different principal, a stronger capability, or an out-of-band approval, not
    merely a different command
enforcement:
  mechanism: The gate's own interface accepts no flag, environment variable, or argument that skips its check
  checks: That refusing the gate on the command line, with no other channel, produces no way through
  not_checked: >
    Whether people work around the gate entirely outside its interface — editing it, disabling it,
    or removing it from the path that runs it; whether the separate escape hatch, once reached, is
    used with the deliberation it is meant to require; and whether the "separate" path actually
    requires a principal or capability the blocked caller doesn't already hold — for an autonomous
    agent that can invoke both paths itself, this is the whole question
evidence: observed-here
evidence_note: >
  In one environment running several checks that protect shared, hard-to-recover state, the gates
  refuse outright with no bypass flag; the override, where one exists, is a separate, explicitly
  acknowledged step rather than an argument next to the failing command. That supports removing the
  inline flag; whether the separate step also requires a different principal or capability than the
  blocked caller holds was not separately verified.
prior_art:
  - >
    GitHub repository rulesets can delegate bypass authority to a designated actor other than the
    person whose push was blocked, through a request that actor acts on, rather than the blocked
    person supplying a flag that clears the rule themselves
  - >
    Git's own documentation for its force-push flag warns that it disables safety checks and can
    discard commits, which is a warning about the same flag it ships
  - >
    Mistake-proofing and guardrail design describe the general principle of making the wrong action
    hard to take by accident, which this is one application of
adds: >
  The general rule that an override needs different authority than the gate's caller, not merely a
  different interface, and the specific reasoning for it: an override reachable by the same
  authority that was just blocked is reached for under time pressure, which is the same condition
  the gate exists to catch
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
An escape hatch can still exist, but what makes it real is a difference in **authority**, not a
difference in interface: a different principal, a stronger capability, or an out-of-band approval
that the blocked caller does not already hold. A different command reachable by the same principal
who was just blocked is not a separate path — it is the same authority, typing more words. The
escape hatch should also require stating plainly what is being bypassed and why, but that
requirement is secondary to who is invoking it.

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

In one environment running several checks that protect shared, hard-to-recover state, the gates
refuse outright with no bypass flag in their own interface; where an override exists, it is a
separate, explicitly acknowledged step, not an argument beside the failing command. That is the
observation this entry generalizes from — not a documented incident of a flag being reached for
under pressure, which was not separately recorded.

## What the practice changes

- **Refuse, don't force.** On an unmet precondition, the gate stops and reports what failed. It does
  not attempt a best-effort workaround on the caller's behalf.
- **No flag in the gate's own interface.** If an override is going to exist, it does not live inside
  the command that just failed.
- **The override requires different authority, not just a different interface.** A different
  principal, a stronger capability, or an out-of-band approval — something the blocked caller does
  not already hold. A different command that the same principal can still invoke alone is not an
  override with teeth; it is the inline flag moved one step away.
- **The gate verifies its own preconditions.** It does not accept a caller's claim that a
  precondition was already satisfied elsewhere; it checks, every time it runs.

## When it helps

Gates that exist specifically to protect against decisions made under pressure — a merge into shared
history, a deploy, an irreversible or hard-to-undo operation. The more likely it is that the person
invoking the gate is in a hurry, the more the adjacency of an override matters.

## What it costs, and when to skip it

This is a real cost, not a theoretical one, for gates outside the fail-closed class described below:
refusing with no reachable override means someone can get genuinely stranded in a real emergency if
the separate escape hatch is slow, undiscoverable, or unreachable at the moment it is needed. For
those gates, the practice only works if the separate path actually exists and actually works when
called on — a gate that is supposed to have a reachable override but doesn't is not a safer gate, it
is an outage waiting for the day something legitimately needs to get through. Building the practice
without building and testing the escape hatch is worse than not building the practice.

**Security, integrity, compliance, and destructive-operation gates are a different class, and this
cost does not apply to them.** They should fail closed with no caller-accessible override at all, by
design. For that class, a gate with no reachable override is not an outage waiting to happen — it is
the control working as intended, and the rest of this practice scopes to gates outside this class.

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
| A narrower flag that skips only some of the gate's checks | Shrinks what gets bypassed but keeps the override reachable by the same principal who was just blocked — sameness of authority, not scope, is the defect |
| A separate command, reachable by the same principal | Adds a step but not a boundary — satisfies "not inline" without satisfying "not the same authority" |
| Time-delay before an inline override takes effect | Can be started early and left running, and still requires no acknowledgment of what is being bypassed |
| No override of any kind, ever | Turns a genuine emergency into a dead end with no path forward, for gates outside the fail-closed class; rejected there for that reason. For the fail-closed class itself, this isn't the rejected alternative — it's the correct design |

## Prior art, and what this adds

GitHub's ruleset bypass mechanism is a published instance of the separate-authority idea: bypass can
be delegated to a designated actor other than the person whose push was blocked, through a request
that actor acts on, rather than the blocked person supplying a flag that clears the rule themselves.
Git's documentation for its own force-push flag is explicit that the flag disables safety checks and
can lose commits — a warning attached to the very flag it ships, which is a smaller version of the
same tension. Mistake-proofing and guardrail design more broadly describe making the unwanted action
hard to reach by accident, which this is one specific application of.

What this adds is the general rule stated independent of any one gate — no override reachable by the
same authority as the blocked caller, regardless of what the gate protects — and the specific
reason: sameness of authority, not distance in the interface, is what turns an override into the
default path, because a caller who can reach it alone will reach it under the exact time pressure
the gate exists to interrupt. No published source examined states that reasoning directly.

## Where this is most likely to be wrong

- **Solo settings with no second party available.** A separate escape hatch that still resolves to
  the same one person, just through a different command, keeps the deliberate-act framing but fails
  the actual bar — different authority — entirely.
- **Emergencies with no reachable approver, outside the fail-closed class.** If the separate path
  depends on someone else being available and they are not, the gate has produced a genuine outage
  rather than a safer default. For the fail-closed class, this is not a wrong case — no reachable
  approver is the intended shape.
- **Low-stakes gates**, where building and maintaining a separate override path costs more than the
  rare bad override it prevents — the practice is meant for gates protecting something expensive to
  recover from, not every check that can fail.
