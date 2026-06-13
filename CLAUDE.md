# KANGARU Agent Starter

This repo takes a builder from "I have an idea" to a working operations agent in
one session. It is meant to be opened in **Claude Code**.

## How to start

When someone opens this folder in Claude Code, greet them briefly and run the
**`new-agent`** skill (in `.claude/skills/new-agent/`). That skill:

1. Checks their model and that `claude -p` works.
2. Interviews them one question at a time (which agent, trigger, fetch, action,
   the one smart step, test credentials, timezone).
3. Scaffolds a new agent folder, writes its `CLAUDE.md` from their answers,
   and inits git.
4. Builds the code file by file, then keeps helping them refine it.

If the builder just says "I want to build an agent" or similar, start the
`new-agent` skill. Always ask one question at a time and explain in plain
language — assume they are not a developer.

## The pattern every agent here follows

```
TRIGGER  →  FETCH  →  THINK (claude -p)  →  ACT
```

The thinking step is the only "intelligent" part and it shells out to the Claude
Code CLI as a subprocess (`claude -p`). There is no model API key anywhere.

## House rules (apply to every agent built here)

- Test credentials only. Never touch real bases, bots, or accounts.
- All secrets live in a `.env` that is never committed. Use `.env.example` for
  placeholders.
- Fail loud: on any error, tell the human in plain language and stop — never
  half-create anything.
- Internal writes (updating a sheet) can be automatic. Anything sent to a real
  person is a draft for human approval.
- Default timezone: Australia/Melbourne unless the builder says otherwise.
