# Using your Fleet Kit crew

You've run the setup interview and your crew is written into `~/.claude/agents/`. Here's how to
work with it. Written for someone who builds real things but doesn't always read the code.

## 1. Your first conversation

Talk to the **architect** by name — it's your one front door. You never talk to the specialists
directly; the architect does that for you.

Ask it for something the way you'd brief a technical co-founder:

> "I want a little tool that turns my meeting notes into a to-do list."

Expect it to **push back before it builds**. It might ask what the one job is, what you want to
see first, and what to leave out of version one. That's the design pass — it's a feature, not
friction. Once you approve the plan, it deploys the crew, gates the result, and comes back with
one answer.

## 2. Meet the crew

Each agent has one job and activates when the work calls for it. You reach them *through* the
architect — the example sentences below are things you'd say to the architect.

- **architect** — your design partner and crew lead. The only agent you talk to. Owns the plan,
  the routing, and the final verdict. → *"Should I even build this, or is it a rabbit hole?"*
- **builder** — implements one scoped task cleanly. → *"Add a dark-mode toggle to the header."*
- **reviewer** — checks changed code for real bugs before it ships. Always runs on code changes.
  → *"Is this change safe to ship?"*
- **tester** — proves a change actually works, with evidence (runs it, screenshots the UI).
  → *"Make sure the export button really downloads the file."*
- **scout** — fast read-only research across your code or the web. → *"Where does this app store
  its settings?"*
- **shipper** — prepares the release path and stops before pushing for your OK. → *"Get this
  ready to ship."*
- **librarian** — tidies your memory and writes end-of-session handoffs. → *"Wrap up and save
  what we decided."*
- **data-safety** — audits anything touching sensitive data (client, user, or personal info).
  → *"Does this leak anyone's data?"*

(If you chose a smaller crew, you'll only have the agents you picked. Re-run setup to add more.)

## 3. How the quality gate works

The gate is automatic — the architect enforces it, you don't have to remember it:

- **Any changed code** → the **reviewer** checks it before you're told it's done.
- **Anything touching sensitive data** → **data-safety** audits it.

A "verdict" is the architect's one scannable reply: what was done, what the gate found, what got
fixed, any residual risk, and the one next step. The specialists' full notes sit below it if you
want them — but you read the verdict, not the transcript.

## 4. Your guardrails

Your setup choice decides what the crew will and won't do without asking:

- **Always ask** — it stops before committing, pushing/publishing, and deleting.
- **Only publishing & deleting** — it commits freely, but stops before push/publish and delete.
- **Never ask** — it proceeds, but still flags hard-to-undo actions.

To change this, re-run `/fleet-kit:fleet-setup` and pick a different guardrail option.

## 5. Re-tuning

Re-run `/fleet-kit:fleet-setup` anytime. The interview pre-fills from your current fleet, so
it's mostly confirmation. Before it overwrites anything, it copies your existing agents aside to
`~/.claude/agents/fleet-kit-backup-<date>/` — nothing is ever deleted. To **add a specialist you
skipped**, re-run setup and include it in the crew step.

## 6. Day-2 patterns

- **Ask for a design pass first.** "Give me a plan before you build" gets you the architect's
  spec to approve once, instead of step-by-step surprises.
- **Ask for plain language.** "Explain what that change means for me, no jargon" — the crew is
  tuned to translate, not just to code.
- **End-of-session handoff.** "Wrap up" sends the librarian to record decisions and write a
  handoff so your next session starts smarter.

## 7. Uninstall

Removing the plugin (`/plugin uninstall fleet-kit@fleet-kit`) leaves your generated crew in
place — it's yours. To remove the crew itself, delete the agent files in `~/.claude/agents/`
(architect.md, builder.md, and the rest). Your memory files are untouched either way.

## 8. Troubleshooting

- **Fleet doesn't appear** — if `~/.claude/agents/` didn't exist before setup, restart Claude
  Code once so the new crew is picked up.
- **Permission prompt on first write** — normal. The setup skill asks once before writing into
  `~/.claude/agents/`; approve it.
- **Bad answer during setup** — just re-run `/fleet-kit:fleet-setup`. It re-tunes from your
  current fleet and backs up before overwriting, so there's no harm in running it again.
