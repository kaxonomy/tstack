# tstack

The agent skills I use every day, cleaned up so anyone can use them. They work
in Claude Code and Codex. Some use [T3 Code](https://github.com/pingdotgg/t3code)
features when they are available and fall back to native tools when they are
not.

## Skills

| Skill | Say | What it does |
|---|---|---|
| `full-send` | "full send" | Takes a change to a merged PR with nobody watching: proves it, reviews it, files it, babysits it, merges it. |
| `full-stop` | "full stop" | Same as `full-send`, but stops when the PR is ready for you to merge. |
| `council` | "council" | Uses Fable 5.2, GPT 6.2 Astra, and Gemini 4 Neon to judge changes made by GPT 6.2 Sol, and loops until all judges agree. |
| `council-opus` | "council opus" | Uses Claude Opus 5.5 instead of Fable 5.2 on the council. |
| `sol-loop` | "sol loop" | Reviews a change with GPT 6.2 Sol in a loop until both agents agree. |
| `neon-loop` | "neon loop" | Reviews a change with Gemini 4 Neon in a loop until both agents agree. |
| `file-pr` | "file a PR" | Files a short PR that leads with the problem, not an inventory of changes. |
| `takeover` | "take over" | Finishes someone else's PR, branch, or agent thread, and keeps their credit. |
| `babysit-pr` | "babysit" | Watches a PR through CI and review bots, and fixes the real findings. |
| `modernize` | "modernize" | Rebases an old PR and decides if it should still exist. |
| `leaving-pr-comments` | (automatic) | Formats comments that an agent posts on your behalf. |

Some skills call others. Install the ones they need too:

- `full-send` uses `sol-loop`, `file-pr`, and `babysit-pr`.
- `full-stop` uses `full-send`.
- `takeover` uses `modernize`, `file-pr`, `babysit-pr`, and `leaving-pr-comments`.
- `file-pr`, `babysit-pr`, and `modernize` use `leaving-pr-comments`.

## Install

```bash
git clone https://github.com/t3dotgg/tstack
cp -R tstack/skills/* ~/.claude/skills/   # Claude Code
cp -R tstack/skills/* ~/.codex/skills/    # Codex
```

## Requirements

- An authenticated [GitHub CLI](https://cli.github.com) (`gh`).
- `python3` for `babysit-pr`'s `pr-wait` script.
- `council` needs Fable 5.2, GPT 6.2 Astra, and Gemini 4 Neon for judging and
  GPT 6.2 Sol for implementation. `council-opus` replaces Fable 5.2 with Claude
  Opus 5.5. `sol-loop` needs a GPT 6.2 Sol reviewer. `neon-loop` needs a Gemini
  4 Neon reviewer. Every model must run at high reasoning. T3 Code supports
  cross-provider delegation with `delegate_task`. Otherwise use a harness
  configured for the required models. Report unavailable models or high
  reasoning without substituting or lowering.

## Make them yours

These are starting points. Edit them. Put other personal settings in your
`CLAUDE.md` or `AGENTS.md`, so you can update the skills without losing them.

## License

[MIT](LICENSE)
