---
name: fleet-setup
description: Build or re-tune your own 8-agent crew — an architect front door plus specialists — through a short plain-language interview, written into your own ~/.claude/agents/. Use when the user runs /fleet-kit:fleet-setup, asks to set up / build / tune / re-tune their crew or fleet, or wants their agents personalized.
---

# Fleet Setup — build the user's crew

You are running the Fleet Kit setup interview. Your job: ask a short, plain-language interview,
then write a tuned agent crew into the user's own `~/.claude/agents/`. The user OWNS the result;
this skill is just the installer. Audience: seasoned vibe coders — fluent in Claude Code, often
with existing CLAUDE.md / memory / lessons files, not necessarily code-readers.

The templates live next to this file in `templates/` (8 files:
architect, builder, reviewer, tester, scout, shipper, librarian, data-safety). You fill their
`{{PLACEHOLDER}}` tokens from the interview answers, then write the filled files to
`~/.claude/agents/`.

Run the steps below IN ORDER. Keep each question to one short screen. Offer the recommended
option first and mark it "(Recommended)". Never dump the templates or the raw file contents at
the user.

---

## Step 0 — Detect (silent, before any question)

Check for existence only — do NOT read contents yet:
- `~/.claude/CLAUDE.md`
- `~/.claude/memory/` (or `lessons.md` / `decisions.md` inside it)
- `git config user.name`
- existing `~/.claude/agents/` (and whether it already has fleet files)

Use `test -e` / `ls` / `git config` via Bash. Say nothing about this step to the user.

## Step 1 — Consent to pre-fill (only if setup was detected)

If Step 0 found a CLAUDE.md or memory files, ask before reading them:

> "You've already got a setup — CLAUDE.md / memory files. May I read them to pre-fill this
> interview? (They're small — this costs almost nothing, unlike a project scan.)"

Options: **"Yes, read my setup (Recommended)"** / **"No — fresh interview"**.

If yes: read those files now and derive a best guess for the user's name, work context, style,
and guardrail preferences. Present each later question PRE-ANSWERED for one-tap confirmation
rather than cold. If no detection happened at all, skip this step and ask each question cold.

## Step 2 — Name

> "Should the crew call you {{best guess}}?"

Best guess order: name from CLAUDE.md → `git config user.name` → ask cold ("What should the
crew call you?"). One tap to confirm, or the user types another. Store as `{{USER_NAME}}`.

## Step 3 — Work

> "What kind of work will your crew mostly help you with?"

Multi-select (let them pick more than one):
- apps/websites for work
- personal tools & automations
- writing / research / content
- exploring

Turn the selection into one or two plain sentences of `{{WORK_CONTEXT}}` (e.g. "You mostly
build personal tools and automations, plus some writing and research."). The optional project
scan in Step 7 can add detail to this later.

## Step 4 — Crew size

Architect + builder + reviewer are ALWAYS included. Ask which of the rest to add:

> "How big a crew do you want?"

- **"Full crew of 8 (Recommended)"** — adds tester, scout, shipper, librarian, data-safety.
  Show each with its one-line job:
  - tester — proves a change actually works, with evidence
  - scout — fast read-only research, "where is X / how does Y work"
  - shipper — prepares the release path, stops before push for your OK
  - librarian — tidies your memory and writes end-of-session handoffs
  - data-safety — audits anything touching sensitive data
- **"Core four (+tester)"** — architect, builder, reviewer, tester.
- **"Pick one by one"** — then walk each remaining specialist (tester, scout, shipper,
  librarian, data-safety) with a yes/no.

Record the final crew as `{{CREW_LIST}}` (a plain comma list of the chosen agents). Only the
chosen agents get written.

## Step 5 — Spend

> "How should your crew spend your Claude subscription?"

- **"Balanced (Recommended)"** — smart where it matters, cheap where it doesn't.
- **"Best brains everywhere"** — sharpest model on every agent; costs the most.
- **"Budget mode"** — cheapest models that still do the job; least sharp.

Map the choice to the model/effort table below to fill each agent's `{{MODEL}}` and
`{{EFFORT}}`.

### Spend tiers → model/effort

| agent      | Balanced        | Best brains     | Budget          |
|------------|-----------------|-----------------|-----------------|
| architect  | opus / high     | opus / high     | sonnet / high   |
| builder    | sonnet / medium | opus / high     | haiku / medium  |
| reviewer   | sonnet / high   | opus / high     | sonnet / medium |
| tester     | sonnet / medium | opus / medium   | haiku / medium  |
| scout      | haiku / medium  | sonnet / medium | haiku / low     |
| shipper    | sonnet / medium | opus / medium   | haiku / medium  |
| librarian  | haiku / medium  | sonnet / medium | haiku / low     |
| data-safety| sonnet / high   | opus / high     | sonnet / medium |

Model values are aliases ONLY: `opus` / `sonnet` / `haiku`. Never write a full model ID.

## Step 6 — Guardrails

> "Stop and ask before hard-to-undo actions?"

- **"Always ask (Recommended)"** — commit + push/publish + delete.
- **"Only publishing & deleting"** — commit freely, ask before push/publish and delete.
- **"Never ask"** — warn: only pick this if you can undo mistakes yourself.

Turn the choice into `{{GUARDRAILS_BLOCK}}` — a short "Hard gates (stop and ask)" block written
in the architect template's voice. Examples:

- Always ask →
  ```
  ## Hard gates (stop and ask — never auto-proceed)
  - `git commit` (any repo) · `git push` / `gh pr create` / tag or force push · deleting files.
  ```
- Only publishing & deleting →
  ```
  ## Hard gates (stop and ask — never auto-proceed)
  - `git push` / `gh pr create` / tag or force push · deleting files. (Commits are fine.)
  ```
- Never ask →
  ```
  ## Hard gates
  - The user has opted out of confirmation gates. Still call out any hard-to-undo action
    before you take it, so they can stop you.
  ```

## Step 7 — Project scan (opt-in, default No)

> "Want the crew tailored to how you already work? I can read through a project once — costs
> tokens (roughly one long conversation per project folder)."

- **"No scan (Recommended)"**
- **"Scan one folder I choose"** — then ASK for the path. Once given, count the files, show a
  rough cost estimate, and CONFIRM before reading anything.
- **"Scan everything"** — confirm scope and cost first.

If scanning: read ONLY README / CLAUDE.md / manifest-level files (e.g. package.json,
pyproject.toml), cap the total read, and distill what you learn into additions to
`{{WORK_CONTEXT}}`. Never read source files wholesale. Default is No — do not scan unless the
user explicitly opts in.

## Step 8 — Style

> "Which line best describes you?"

- non-coder who builds real things
- somewhere in between
- developer who wants leverage

Pre-select from detection if you read the user's setup. Turn the choice into one plain sentence
of `{{STYLE_LINE}}` describing how the crew should pitch explanations (e.g. "You build real,
shippable things but don't always read the code — explain changes in plain language, not
jargon.").

## Step 9 — Preview + write

Show a SHORT preview before writing anything:
- the crew list (chosen agents)
- each agent's model
- the guardrail choice
- the name + one-line work/style context

On confirm, write the chosen filled templates to `~/.claude/agents/`:

1. **Fill placeholders.** For each chosen template, replace every `{{PLACEHOLDER}}`:
   `{{USER_NAME}}`, `{{WORK_CONTEXT}}`, `{{STYLE_LINE}}`, `{{GUARDRAILS_BLOCK}}`,
   `{{CREW_LIST}}`, `{{GENERATED_DATE}}` (today's date), and each agent's `{{MODEL}}` /
   `{{EFFORT}}`. Leave NO `{{...}}` token behind — grep the output to be sure.
2. **Prune the architect for the chosen crew.** The `architect.md` template references every
   possible specialist, but the user may have chosen a smaller crew (e.g. "Core four"). An
   architect told to route to an agent that isn't installed silently weakens the gate. Conditional
   lines in `architect.md` are marked with an HTML comment `<!-- crew-conditional: <agent> -->`.
   For each such line, look at the `<agent>` named in its marker:
   - `builder` and `reviewer` are ALWAYS installed — they carry no marker; never touch their lines.
   - If `<agent>` IS in the chosen crew: KEEP the line, but strip the trailing
     `<!-- crew-conditional: ... -->` comment so it doesn't ship to the user.
   - If `<agent>` is NOT in the chosen crew: DELETE the entire line (the routing-map bullet or
     the gate bullet it sits on), comment and all.
   - **Special case — data-safety gate.** The line marked `<!-- crew-conditional: data-safety (gate) -->`
     is the sensitive-data gate bullet. If data-safety is NOT chosen, do NOT just delete it —
     REPLACE that whole bullet with, verbatim:
     `- Work touched sensitive data and no data-safety specialist is installed → you (the architect) must run the sensitive-data check yourself before delivering, and recommend the user re-run /fleet-kit:fleet-setup to add data-safety.`
     (The routing-map line marked `<!-- crew-conditional: data-safety -->`, without `(gate)`, is
     just a router entry — delete it normally when data-safety isn't chosen.)
   - `shipper`: if not chosen, delete BOTH its routing-map line and the gate line marked
     `<!-- crew-conditional: shipper -->` (the pre-ship imported-but-untracked check). Nothing
     else depends on them.
   After pruning, grep the written `architect.md` for `crew-conditional` — ZERO matches must
   remain (every marker is either stripped or deleted with its line).
3. **Overwrite protection.** If `~/.claude/agents/` already has a file with the same name,
   ASK first. Before overwriting, copy the existing same-named files aside to
   `~/.claude/agents/fleet-kit-backup-<today's-date>/`. NEVER delete anything.
4. **Write** the filled files to `~/.claude/agents/`.
5. **Tell the user:** the first write may trigger a one-time permission prompt (normal), and if
   `~/.claude/agents/` didn't exist before, restart Claude Code once so the new fleet is picked
   up.

Write only inside `~/.claude/agents/` (plus the backup subfolder). Never write anywhere else.
Never delete a file.

## Step 10 — Closing message

Give the 3-line quickstart and point to USAGE.md:
- Talk to the architect by name — it's your one front door; you never talk to specialists.
- It challenges the idea, then deploys the crew and gates the result.
- The review gate is automatic on any changed code.
- "See USAGE.md for the full guide. Re-run `/fleet-kit:fleet-setup` anytime to re-tune."

Re-running this skill later = re-tune: same flow, pre-filled from the current fleet.
