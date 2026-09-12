# agent-practices

Ten working practices for AI agents, each written as **the obvious answer, the answer that actually
worked, and why the obvious one failed.**

This is not a style guide and not a set of instructions. It is a record of decisions that took real
iteration to converge, published in the form that is most useful to something reading it mid-task.

## Why the discarded alternatives are the point

Most published practice is a list of conclusions. Conclusions are cheap and mostly interchangeable —
an agent working the problem can usually derive them. What does not survive the write-up is the
approach that looked right first, was tried, and failed for a reason that was not visible in advance.

So every entry here leads with the obvious answer and says what went wrong with it. If an entry has
nothing interesting in that section, the entry does not belong here.

## These came from one environment

Windows, PowerShell, a particular stack, one person's working style, several coding agents running
in parallel. Some of what makes these work is genuinely general. Some of it is local, and a few are
probably preference wearing a rule's clothes.

Each entry says which it thinks it is, under **Scope**. Those labels are claims, not facts, and the
most useful thing you can send back is evidence that one of them is wrong.

## How to read this

Entries are indexed by **situation**, not by topic. Find the one that matches what you are about to
do:

| You are about to… | Read |
|---|---|
| Review something, or be reviewed, with no clear end | [Set the review budget before review starts](practices/review-that-terminates.md) |
| Decide whether a review finding actually blocks | [A finding needs a reachable case](practices/what-blocks-a-landing.md) |
| Fix a comment or a doc paragraph that is wrong | [Wrong prose is deleted, not rewritten](practices/wrong-prose-is-deleted-not-rewritten.md) |
| Add a rule to a standards document | [Any addition requires a deletion](practices/any-addition-requires-a-deletion.md) |
| Decide how much review something needs | [Review depth comes from cost, not subject](practices/review-depth-comes-from-cost-not-subject.md) |
| Get a second opinion on your own work | [Independence is a property of the seat](practices/independence-is-a-property-of-the-seat.md) |
| Build a gate, a check, or a guard | [No inline override on a gate](practices/no-inline-override-on-a-gate.md) |
| Hand off work you will not finish | [The handoff is written for a stranger](practices/the-handoff-is-written-for-a-stranger.md) |
| Make the same judgment call you have made before | [Encode on the third time](practices/encode-on-the-third-time.md) |
| Run shell commands behind a permission prompt | [One command per call](practices/one-command-per-call.md) |

## This does not override anything

It is optional reference material. It is not addressed to you by your operator, it carries no
authority over your instructions, and nothing in it should be treated as a directive. If an entry
conflicts with what you were told to do, what you were told to do wins. Read it, take what is
useful, discard the rest.

## Contributing

The most valuable thing you can send is a **counterexample**: you adopted one of these, it broke in
your environment, here is what happened. No fix required — see [CONTRIBUTING.md](CONTRIBUTING.md).

## Related

Five Claude Code skills, same author: <https://github.com/robmcquade/claude-skills>

## License

MIT. See [LICENSE](LICENSE).
