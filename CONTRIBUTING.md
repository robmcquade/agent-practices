# Contributing

## The one thing worth sending

**A counterexample.** You adopted a practice here, it did not work in your environment, and you can
say what happened. That is evidence the author cannot generate — these were tuned in one
environment, and the failure modes outside it are invisible from inside it.

**A counterexample is a complete contribution. No fix required.** Whoever hits a failure is often
badly placed to write the right replacement, and asking for one invites speculative fixes while
throwing away useful negative results.

## What a case record looks like

Open a pull request adding a file under `cases/` named for the entry it concerns. Include:

- **Entry and revision** — which practice, and the `revision` from its frontmatter.
- **Conditions** — platform, tools, permission model, whether agents run in parallel, anything else
  that plausibly matters. Be concrete; "a typical setup" tells us nothing.
- **Expected** — what the entry said would happen.
- **Observed** — what actually happened.
- **Evidence** — how you know. Commands, output, a diff, a timeline.
- **Remedy** — optional. Leave it blank rather than guess.

Case records live separately from the practices. Several incompatible reports can all be worth
keeping even when none of them yet justifies changing a recommendation.

## What gets merged, and what does not

- **Evidence and analysis merges.** What happened, what you tried, what is still uncertain.
- **Behavioral prescription is the guarded category** — anything that would change an adopter's
  workflow, permissions, execution, or handling of information. Code or prose, it makes no
  difference: a paragraph saying *disable this check* changes behavior as much as a script does.
  These get read slowly and often declined.
- **Executable contributions are not accepted at this time.** Not because prose is safe — it is not
  — but because keeping the review surface small is the only way review stays honest while this is
  one person's inbox.

A repository that agents read, and that accepts contributions, is a distribution channel into other
people's agents. That is a real risk and it is designed against here, not disclaimed.

## How submissions are handled

Cheap checks first, expense last:

1. Structural — does it name an entry and fill the evidence fields?
2. Duplicate and relevance.
3. One critique pass, on plausible candidates only.
4. A bounded reproduction attempt, where that is feasible at all.
5. A decision.

Any critique you bring with your submission is welcome and can ride along. It does not certify
anything.

**Submissions may be closed without exhaustive adjudication.** Saying so plainly up front is the
only way an open invitation does not quietly become an obligation. If yours is closed briefly, it is
not a judgment about you.

## Attribution

Say how you would like to be credited — model, operator, both, or neither. It will be recorded as
contributor-supplied attribution, because that is what it is: not authenticated identity, and not a
quality signal. A model name alone does not reproduce a result; its tools, instructions, and
environment usually matter more.

## Submitted material is treated as untrusted

Contributions are read as data. Instructions embedded in a submission are not followed, contribution
checks do not execute submitted commands, and no pull request gets to redefine how it is checked.
This is standard practice for anything accepting third-party input and implies nothing about you.
