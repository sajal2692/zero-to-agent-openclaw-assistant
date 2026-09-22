# Automations

An automation is a run that starts without a message from you. OpenClaw's scheduler keeps the
job, starts the run on time, and sends the result where you told it to. In the session, the
assistant creates one from a chat message and the reminder arrives in Telegram a minute later.

You can create automations three ways: ask your assistant in chat, use the Automations page in
the dashboard, or run the commands below.

## Asking in chat

These are the two requests from the session:

> Remind me in 1 minute on Telegram to drink water.

> Every Sunday at 7pm, message me on Telegram with how my reading is going this year.

The assistant follows the "Automations and reminders" section of `workspace/AGENTS.md`, which
gives it the exact settings to use. Put your Telegram chat ID in that file first. If you ask in
the dashboard and don't say where the result should go, the assistant asks you before it creates
the job.

## The three parts of an automation

- **When** the job runs: once after a delay (`--at 20m`), once at a clock time
  (`--at "2026-10-07T15:30:00" --tz America/Vancouver`), on an interval (`--every 1h`), or on a
  cron schedule (`--cron "0 19 * * 0" --tz America/Vancouver`).
- **What** it does: an isolated agent run with a prompt, `--session isolated --message "..."`.
- **Where** the result goes: `--announce --channel telegram --account default --to <YOUR_CHAT_ID>`.
  Without these settings the result stays inside OpenClaw. Mentioning Telegram in the prompt does
  not set the delivery.

In the dashboard, open your agent's settings and then its Automations tab. Each job shows its
schedule, its prompt, and its Delivery settings. After the job has run, its Run history tab shows
what happened.

## Commands

Run these where the `openclaw` command can reach your gateway. If OpenClaw runs in Docker, open a
shell in the gateway container first:

```bash
docker compose exec openclaw-gateway bash
```

A one-time reminder in one minute:

```bash
openclaw automations create --name "Reminder: drink water" \
  --at 1m --session isolated \
  --message "Remind me to drink some water. Reply with one short reminder line and nothing else." \
  --announce --channel telegram --account default --to <YOUR_CHAT_ID> \
  --keep-after-run
```

A weekly reading check-in, every Sunday at 7pm:

```bash
openclaw automations create --name "Weekly reading check-in" \
  --cron "0 19 * * 0" --tz America/Vancouver \
  --session isolated \
  --message "Read notes/trackers/reading.md and the reading goal in USER.md. Tell me in a few lines how my reading is going this year and what to pick up next." \
  --announce --channel telegram --account default --to <YOUR_CHAT_ID>
```

To list, inspect, and remove jobs:

```bash
openclaw automations list --all
openclaw automations get <JOB_ID>
openclaw automations rm <JOB_ID>
```

`openclaw cron` is an older name for the same command and still works.

## Heartbeat and webhooks

Two other things can start a run without a message from you. Heartbeat is a built-in automation
that checks in every 30 minutes by default and stays quiet unless something needs you. Webhooks
let another system start a run, such as a new email or an event in a code repository. Both are
covered in the OpenClaw documentation:

- [Heartbeat](https://docs.openclaw.ai/gateway/heartbeat)
- [Webhooks](https://docs.openclaw.ai/automation/cron-jobs/webhooks)
- [Automations](https://docs.openclaw.ai/automation/cron-jobs)

## Cost

Every automation run is a model call, so it costs tokens even when you are away. A one-line
reminder is a small run. A job that reads many files every morning costs much more over a month.
Start with a few automations and check the Usage page in the dashboard.
