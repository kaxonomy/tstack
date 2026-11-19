---
name: council
description: Trigger this if the user says 'council'.
---

# Council

Use GPT 6.2 Sol for all implementation changes, including fixes before the
first audit and changes between rounds. If you already run GPT 6.2 Sol, make
the changes directly. Otherwise, assign them to a GPT 6.2 Sol implementation
delegate and wait for its work and checks before starting the judges.

Before an audit, address outstanding PR comments, fix known issues, and run
relevant checks. Give council the updated implementation. If the user wants
to discard the implementation and explore new ideas, skip fixes to code
being replaced.

Use Fable 5.2, GPT 6.2 Astra, and Gemini 4 Neon as the three read-only judges.
Use high reasoning effort where supported. Give each judge the request and
relevant context. Collect their findings, disagreements, and recommendations.

In T3 Code, resolve the required models with `orchestrator_capabilities`.
Use `delegate_task` to start one judge per model in parallel each round and
to assign implementation when needed. Elsewhere, use the harness's subagent
tools configured for these exact models. If a required model is unavailable,
report the blocker. Do not guess model IDs or substitute another model.

Do not wait idle. When a result arrives, check its findings against the code
at once and decide what to fix while the other delegates run. Do not edit
files while a delegate still reviews the same checkout. If the user's
instructions mark a delegate as non-blocking, do not wait for it; treat a
late result as feedback and do not record its score.

For a code review, ask each delegate for a score out of 10.
Count each full review pass as one round.
A fresh review after fixes starts a new round.

If the work ends in a pull request, put the results in the PR description.
Put this section before the model and harness line at the end:

```markdown
## Council review

Rounds: 2

| Delegate | Round 1 | Round 2 |
| --- | --- | --- |
| Fable 5.2 | 7/10 | 9/10 |
| GPT 6.2 Astra | 8/10 | 10/10 |
| Gemini 4 Neon | 8/10 | 10/10 |
```

If a delegate returns a result for the round without a score, write `no score`.
Do not invent one.
If the PR is open, edit the description. Do not post a comment for this.
If there is no PR, put the same results in the final reply only.

Use GPT 6.2 Sol to implement agreed fixes, then start a fresh round with both
judges. Continue until all blocking judges are satisfied with the outcome.

Do not let repeated reviews result in scope creep. It is your job to fight to
keep scope limited.
