# KANGARU Agent Starter

Bring your laptop, clone this repo, and build a working ops agent in one session.
You don't need to be a developer — Claude Code walks you through every step.

---

## What you'll end up with

```
your-agent/
  CLAUDE.md     # your agent's brief, written from your interview answers
  README.txt    # plain-language: what it does, how to run it
  code/         # the bot and its files
  context/      # sample inputs and notes
```

Built on one pattern:  **Trigger → Fetch → Think (claude -p) → Act**

---

## One-time setup (do this before the session if you can)

1. **Install Claude Code.** Easiest is the desktop app (Code tab), no terminal
   needed. Or install the CLI:
   ```
   npm install -g @anthropic-ai/claude-code
   ```
   (The CLI route needs Node.js 18+. The native installer needs nothing — see
   the Claude Code docs.)

2. **Log in and pick your model.**
   ```
   claude login
   ```
   You need a paid plan (Pro, Max, Team, or Enterprise). The free tier won't run
   Claude Code.

3. **Check it works.**
   ```
   claude -p "say hello"
   ```
   If that prints text, you're ready. If it asks you to log in, run
   `claude login` again.

---

## Start building (at the session)

1. **Clone this repo** (or download the ZIP and unzip it):
   ```
   git clone <REPO-URL>
   cd kangaru-agent-starter
   ```
2. **Open Claude Code in the folder:**
   ```
   claude
   ```
3. **Turn on auto-accept** so it can create files: press **Shift+Tab**.
4. **Say:** `I want to build a new agent.`
5. **Answer the questions.** It scaffolds everything, writes your agent's brief,
   inits git, and builds the code with you. Then you just keep talking to it.

---

## Need an idea? Pick one of these

Lead researcher · Meeting preparer · Competitor monitor · Support triage ·
Proposal drafter · Churn detector · Content repurposer · Invoice processor ·
Onboarding assistant · Partnership scout — or describe your own.

Easiest to finish in one sitting: **Content repurposer** (no integrations) or
**Meeting preparer** (everyone has a calendar).

---

## The two rules that keep this safe

- **Test credentials only.** Never use a real Airtable base or Telegram bot.
- **Secrets stay in `.env`,** which is never committed. The real keys and the
  production deploy are the organiser's job — your finish line is a working demo
  against test data.
