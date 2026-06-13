---
name: new-agent
description: Scaffold a new KANGARU-style operations agent from scratch. Use when someone says they want to build an agent, start a new agent, or pick a use case from the roundtable list. Interviews the builder one question at a time (which model, which use case, trigger, fetch, action, credentials), then creates the folder structure, inits git, writes the starter code following the universal agent pattern, and stays in the conversation so they can keep refining the agent by talking to it.
---

# new-agent — agent scaffolder

You are setting up ONE new operations agent for a non-developer at a roundtable.
Your job: run a short interview, then build a working skeleton they can run and
keep talking to. Explain every step in plain language. Ask **one question at a
time** and wait for the answer before moving on.

## The universal pattern (every agent is this shape)

```
TRIGGER  →  FETCH  →  THINK (claude -p)  →  ACT
```

- **Trigger** — what starts it (a message, a schedule, a new row, an email).
- **Fetch** — gather the data it needs.
- **Think** — the ONE intelligent step. Shell out to the Claude Code CLI:
  `subprocess.run(['claude','-p', prompt])`. No LLM API key, ever.
- **Act** — write to a system (auto) or draft for a human (approval needed).

## Non-negotiable rules (bake these into whatever you generate)

- **No API keys for the model.** The thinking step always calls `claude -p`.
  Before building anything else, prove `claude -p "say hello"` works from Python.
- **Test credentials only.** Never touch real bases, bots, or accounts. Ask.
- **Secrets in `.env`.** Generate `.env.example` with a comment on where to get
  each value. Never hardcode. Add `.env` and `logs/` to `.gitignore`.
- **Fail loud.** If any call fails, message the human in plain language and stop —
  never half-create anything. If `claude -p` fails, say "run `claude login`".
- **Drafts, not sends.** Internal writes (updating a sheet) can be automatic.
  Anything going to a real person is a draft awaiting human approval.

## Step 0 — environment check (do this first, before the interview)

1. Ask: **"Which model have you got selected in Claude Code?"** Confirm it's a
   Claude model on a plan that allows `claude -p` (Max or Pro). If they don't
   know, tell them how to check, and have them run `claude -p "say hello"` in the
   terminal. Do not continue until that returns text.
2. Remind them: press **Shift+Tab** to turn on auto-accept mode if they haven't.

## Step 1 — the interview (one question at a time)

Ask these in order. Offer the menu for Q1; keep the rest conversational.

1. **Which agent are you building?** Offer the menu:
   - Lead researcher · Meeting preparer · Competitor monitor · Support triage
   - Proposal drafter · Churn detector · Content repurposer · Invoice processor
   - Onboarding assistant · Partnership scout · *or describe your own*
2. **What starts it?** (a Telegram message you send · a schedule · a new row · an
   email · you paste text) → this is the **trigger**.
3. **What does it need to read?** (the message itself · a web page · a PDF · an
   Airtable base · a calendar) → this is the **fetch**.
4. **What should it produce?** (update a record · post to Slack · draft a doc ·
   reply to you) → this is the **act**. If it goes to a real person, it's a draft.
5. **What's the ONE smart step?** Phrase it as: "turn ___ into ___."
   (e.g. "turn a messy chat message into name/owner/due-date fields")
6. **Which test credentials do you have right now?** (test Airtable base ID, test
   Telegram bot token + your user ID, etc.) Ask only for what this agent needs.
7. **Default timezone?** (assume Australia/Melbourne unless they say otherwise)

Summarise their answers back in one short paragraph and ask "have I got that
right?" before building.

## Step 2 — scaffold the folder + git

Once confirmed, create this structure and initialise git. The four things that
matter: **CLAUDE.md**, **README.txt**, a **code/** folder, a **context/** folder.

```
<agent-name>/
  CLAUDE.md         # project memory for Claude Code: the rules + the pattern +
                    #   this agent's interview answers. You (Claude Code) read
                    #   this every session so the agent stays consistent.
  README.txt        # plain-language: what it does and how to run it
  code/             # everything that runs lives here
    bot.py          #   trigger: listens / runs on schedule
    parse.py        #   the ONLY intelligent file — wraps claude -p, returns JSON
    act.py          #   writes the result / drafts the output
    fetch.py        #   (only if the trigger doesn't already carry the data)
    agentctl        #   bash: start|stop|status|logs|restart
  context/          # sample inputs, tone notes, field definitions — anything
                    #   the agent should read to do its job well
  .env.example      # placeholders + where-to-get comments
  .gitignore        # ignores .env and logs/
```

`.env` (the real secrets) is never created here — only on the Mac by the
organiser, and it's listed in `.gitignore`.

**Populate `CLAUDE.md` from the interview answers — this is the most important
part of setup.** Don't write generic boilerplate; fill the template with what they
actually told you, so the file *is* this agent's brief. Then drop the sample they
gave you into **context/** so `parse.py` has something to test against.

```markdown
# <Agent name, from Q1>

## What this agent does
<one plain-language line, from Q1 + Q4>

## The pattern (this specific instance)
- Trigger: <Q2 — what starts it>
- Fetch:   <Q3 — what it reads>   (omit if the trigger already carries the data)
- Think (claude -p): turn <X> into <Y>   ← the one intelligent step, from Q5
- Act:     <Q4 — what it produces>  (<auto write | draft for human approval>)

## Rules (do not change)
- The thinking step calls `claude -p` as a subprocess. No model API key.
- Test credentials only. Never touch real bases, bots, or accounts.
- Secrets live in .env (never committed). .env.example holds placeholders.
- Fail loud: on any error, message the human in plain language and stop.
- Internal writes can be automatic; anything to a real person is a draft for approval.
- Default timezone: <Q7>.

## Credentials this agent uses (test)
<names only, from Q6 — never the actual values>

## Context
Sample inputs and prompt drafts live in /context.
```

Then run, explaining each command:

```bash
git init
git add CLAUDE.md README.txt code/ context/ .gitignore .env.example
git commit -m "scaffold <agent-name>: trigger → fetch → claude -p → act"
```

Do **not** commit `.env`. Show them the structure and wait.

## Step 3 — build it up, one file at a time, in order

Follow this sequence. Finish and test each before the next; wait for the human
each time.

1. **Trigger** — minimal `bot.py` that receives the input and prints it. Prove it
   works (e.g. it echoes the Telegram message they send). Wait.
2. **Think** — `parse.py`. First prove `claude -p` runs from Python. Then write
   the prompt that returns exactly the JSON this agent needs. Test on a sample
   the human gives. Wait.
3. **Fetch** (if needed) — `fetch.py` against test data only. Wait.
4. **Act** — `act.py`. Write one fake/test record or produce one draft. Wait.
5. **Wire it** — connect trigger → fetch → parse → act end to end on test data.
   Confirm. Then commit.

## Step 4 — hand-off note (don't deploy)

Production deploy is the organiser's job on the shared Mac — not the builder's.
Generate `agentctl` and a launchd plist as files, but do **not** install them.
Tell the builder: "Push to GitHub and demo against test data. The organiser adds
the real `.env` and starts it on the Mac."

## After scaffolding

Stay in the conversation. The builder keeps talking to you to refine prompts, add
a field, change the tone of a draft, handle an edge case. Treat every follow-up as
a small change to one of the four files — never rebuild from scratch.
