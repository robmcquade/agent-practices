---
name: one-command-per-call
title: One command per call, and pass the directory as a flag
situation: An agent runs shell commands behind a per-command human approval prompt.
revision: 1
scope: >
  Written for agent-driven shell execution where a human or a policy layer approves each command
  before it runs, evaluated largely by the command's shape. Observed against one such boundary's
  classifier; a boundary that evaluates intent or semantics rather than shape may not behave the
  same way, and the specific shapes that escalate are a property of that classifier, not a law of
  shells in general.
capabilities:
  - A permission layer that assesses each shell call before it runs
  - Commands whose target directory can be passed as a flag rather than set with a preceding change
    of directory
enforcement:
  mechanism: >
    The approval layer itself — it evaluates each command's shape and decides whether to run it or
    escalate to a human.
  checks: >
    Whether the literal first token is a recognized binary, whether the command contains chaining or
    piping operators, and whether it leads with a directory change or a variable assignment instead
    of the binary.
  not_checked: >
    What the command actually does once its shape clears the check. Shape says nothing about the
    arguments — a routine-looking command with dangerous arguments passes the same classifier a
    genuinely routine one does.
evidence: observed-here
evidence_note: >
  Discovered empirically, through commands being escalated repeatedly, not derived by reasoning
  about the boundary in advance.
prior_art:
  - >
    Agent sandboxing and execution projects publish argv allowlisting, restrictions on pipes and
    redirection, and working-directory confinement as security controls on what an agent may run.
adds: >
  The same shapes reframed as an interruption-reduction technique from the agent's side of the
  boundary, with the requirement that the boundary's own controls stay fully intact attached
  directly to the technique.
---

# One command per call, and pass the directory as a flag

**The obvious answer:** chain commands together — `cd`, then the real command, joined with `&&` or
`;` — to get more done per approval and cut down on round trips.

**The practice below is about reducing how often a human is interrupted. It must never be read as
reducing what the approval boundary catches — the two are measured separately, and a technique that
improves the first at the cost of the second is a harm, not a win.**

**What it misses:** a chain is exactly the shape a per-command boundary can't cheaply assess. It
either has to evaluate every command in the chain to approve the whole thing, or escalate the whole
thing to a human — and if it escalates, chaining hasn't reduced interruptions, it's produced one
interruption with a chain-shaped command attached to it instead of several plain ones. Leading with
`cd` or a variable assignment is worse: it pushes the recognizable binary out of the first-token
position the classifier keys on, which is often enough by itself to force escalation regardless of
how routine the rest of the command is.

**The practice:** one shell command per tool call. The literal first token is a recognizable binary.
The working directory is passed as a flag on that binary (`git -C <path>`, `npm --prefix <dir>`,
`gh --repo <owner/repo>`) instead of a preceding `cd`. When several commands are genuinely needed,
issue them as separate calls — in parallel where they don't depend on each other — instead of joining
them with `;` or `&&`.

This is not something reasoning it through in advance would get you to. It was found by hitting the
escalation repeatedly and noticing which command shapes triggered it and which didn't.

## When to consult this

You're an agent about to issue a shell command behind an approval boundary and are tempted to batch
several steps into one call to save a round trip — or you're the one setting up that boundary and
trying to understand what's driving its escalation rate.

## What happened

Commands shaped as chains, or led by `cd` or an assignment, were escalated to a human approval
prompt consistently, independent of how routine the underlying operations were. The same operations,
issued as separate calls with a recognizable binary first and the directory passed as a flag, were
not escalated at the same rate. The pattern showed up from being blocked, not from inspecting the
boundary's logic ahead of time.

## What the practice changes

- One command, one call.
- A recognizable binary as the literal first token — never `cd`, never a variable assignment.
- The working directory as a flag on that binary, not a preceding change of directory.
- Several needed commands become several calls, not one joined statement.

## When it helps

Any setup with a human or a policy layer approving shell calls one at a time, where the agent issues
many small, mostly-routine commands — the interruption rate tracks command shape more than it tracks
what the commands actually do.

## What it costs, and when to skip it

**Getting the metric wrong is the whole risk here.** The success metric is fewer *unnecessary*
prompts with the boundary's own checks still fully intact — never a raw reduction in prompt count. A
command shaped to look routine while doing something the boundary would otherwise have flagged is
exactly what this practice must not produce, and adopting it without watching for that is worse than
not adopting it at all. There's also a smaller, ordinary cost: more calls in the everyday case, since
work that used to be one chained statement is now several separate ones, each waiting its turn.

Skip enforcing this shape where there's no per-command approval step to interrupt in the first place
— fully sandboxed execution with no human or policy layer in the loop has nothing here to economize
on.

## How you can tell whether it worked

Two things, checked together, never just one: fewer human interruptions on genuinely routine
operations, *and* a command deliberately shaped to be dangerous still escalates. Checking only the
first number is how a boundary gets quietly worn down while everything looks like it's improving.

## Alternatives considered and rejected

| Alternative | Why it was rejected |
|---|---|
| Chain commands with `&&` or `;` for fewer round trips | The chain can't be assessed as a whole; the boundary either approves it blind or escalates the entire thing |
| Lead with `cd <dir>` then run the real command | Pushes the binary out of the first-token position the classifier keys on, forcing escalation regardless of the rest of the command |
| Wrap several steps in a script file and invoke that | Moves the actual chain out of the boundary's view entirely, defeating the check rather than satisfying it |
| Ask for a standing allowlist rule to cover the batch | That's a lever the operator pulls on the boundary, not something the agent can grant itself — and every widening of it is the exact risk the metric above exists to catch |
| Put multiple steps in one multi-line here-string | Still one opaque blob to a shape-based check; fails the same way chaining does |

## Prior art, and what this adds

Agent sandboxing and execution projects already publish argv allowlisting, pipe and redirection
restrictions, and working-directory confinement as security controls on what an agent is permitted
to run. What this adds is the same shapes read from the other side of the boundary — as a technique
for reducing how often the agent interrupts a human — with the requirement that the underlying
control stay intact attached directly to the technique, not left as an assumption someone else has to
supply.

## Where this is most likely to be wrong

- **A boundary that classifies by intent or semantics rather than by shape** — these specific shapes
  may not matter there, or may not transfer to whatever it does check instead.
- **No per-command approval step at all** — sandboxed execution with nothing in the loop to interrupt
  has nothing for this to reduce.
- **Read as a security practice rather than an interruption-reduction one.** It is not a substitute
  for the boundary's own controls, and using it as if it were is the misuse the cost section exists
  to name.
