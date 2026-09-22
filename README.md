# Build a Personal Assistant on OpenClaw

This repository goes with my October 7, 2026 O'Reilly session, **Build a Personal Assistant on
OpenClaw**, part of *Zero to Agent in 30*.

## What's in the repository

| Path | What it holds |
|---|---|
| `workspace/` | The assistant's workspace, ready to copy into OpenClaw |
| `workspace/AGENTS.md` | Operating rules, local settings, and the instructions for reminders |
| `workspace/SOUL.md` and `IDENTITY.md` | Personality and name. The sample assistant is called Alfred. |
| `workspace/USER.md` and `MEMORY.md` | Who the assistant works for, and its long-term memory |
| `workspace/notes/` | Sample tasks, daily notes, a weekly review, and a reading tracker |
| `workspace/skills/` | `books-tracker` for the reading list and `notes-tasks` for the task list |
| `docs/automations.md` | Reminders and scheduled jobs, with the exact commands |

Start with `SOUL.md` and `USER.md` to see how the assistant is set up, then read the
"Automations and reminders" section of `AGENTS.md`.

## Requirements

- OpenClaw, which is free and open source.
- A machine that stays on, if you want the assistant to reach you while your laptop is closed.
  A small VPS, a mini PC at home, or a spare laptop all work. Your own laptop is fine for trying
  it out.
- A model for it to use: an existing Claude Code or Codex login, or an API key from a model
  provider. Model usage costs money.
- A Telegram account, and a bot token from Telegram's @BotFather.

## Setup

1. **Install OpenClaw** and go through onboarding. On macOS or Linux:

   ```bash
   npx openclaw@latest
   ```

   The [getting started guide](https://docs.openclaw.ai/start/getting-started) covers the other
   install options. For a VPS with Docker, follow the
   [deployment guide from my full course](https://github.com/sajal2692/openclaw-oreilly-live-course/blob/main/deployment/linux-vps-guide.md).

2. **Connect Telegram.** Add your bot, send it a message, then approve the pairing request:

   ```bash
   openclaw channels add --channel telegram --token <BOT_TOKEN>
   openclaw pairing list telegram
   openclaw pairing approve telegram <CODE>
   ```

   The [Telegram setup guide](https://docs.openclaw.ai/channels/telegram/setup) has the details.

3. **Copy the workspace in.** OpenClaw keeps the agent's workspace in `~/.openclaw/workspace`.
   If you already have one, back it up first.

   ```bash
   git clone https://github.com/sajal2692/zero-to-agent-openclaw-assistant.git
   cp -R ~/.openclaw/workspace ~/.openclaw/workspace.backup
   cp -R zero-to-agent-openclaw-assistant/workspace/. ~/.openclaw/workspace/
   ```

4. **Put your own details in.**
   - In `USER.md`, change the name, timezone, and goals to yours.
   - In `AGENTS.md`, set the timezone in the Local environment list, and replace every
     `<YOUR_CHAT_ID>` with your numeric Telegram chat ID. To find it, send your bot a message
     while `openclaw logs --follow` is running. The Telegram setup guide lists other ways.

5. **Open the dashboard** and start a new session:

   ```bash
   openclaw dashboard
   ```

## Credits

- [OpenClaw](https://github.com/openclaw/openclaw) and its
  [documentation](https://docs.openclaw.ai).
