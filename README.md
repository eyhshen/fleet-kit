# Fleet Kit

Your own AI crew, tuned to you in a 3-minute interview.

Fleet Kit is a Claude Code plugin that gives you an **8-agent crew** — one architect you talk
to, plus specialists who work behind the curtain (builder, reviewer, tester, scout, shipper,
librarian, data-safety). You never micro-manage them. You talk to the architect like a founder
talks to a technical co-founder; it challenges the idea, deploys the right specialists, gates
the result, and hands you one answer.

The plugin ships **no active agents**. It ships one setup skill and a set of agent *templates*.
The setup interview writes a tuned crew into your own `~/.claude/agents/`. You own the result —
the plugin is just the installer.

## Install

```
/plugin marketplace add eyhshen/fleet-kit
/plugin install fleet-kit@fleet-kit
/fleet-kit:fleet-setup
```

The third command runs the setup interview and builds your crew.

## What the interview asks (and why)

- **Your name** — so the crew addresses you, not "the user".
- **Your work** — apps, personal tools, writing, exploring — so the crew's context fits you.
- **Crew size** — the full 8, a core four, or pick specialists one by one.
- **Spend** — Balanced / Best brains / Budget — maps each agent to a model so you spend your
  Claude subscription the way you want.
- **Guardrails** — whether the crew stops and asks before commit / push / delete.
- **Optional project scan** — opt-in, with a cost estimate first, to tailor the crew to how you
  already work.
- **Style** — non-coder / in-between / developer — so explanations land at the right level.

If you already have a `CLAUDE.md` or memory files, the interview offers to read them and
pre-fill every answer, so it's mostly one-tap confirmation.

## Privacy

Nothing leaves your machine except normal Claude API calls. The interview reads only what you
allow. The project scan is **opt-in** and shows a rough token cost estimate before it reads
anything. The crew it writes lives in your own `~/.claude/agents/` — yours to edit or delete.

## Learn more

See [USAGE.md](./USAGE.md) for the full how-to: meeting the crew, how the quality gate works,
your guardrails, re-tuning, and troubleshooting.
