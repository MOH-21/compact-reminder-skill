# compact-reminder

Claude Code skill that nudges you to `/compact` during long sessions — with push notifications and automatic silence during plan mode so it never breaks your focus.

## Install

```bash
npx skills add MOH-21/compact-reminder
```

## Usage

```
/auto-compact          # default: every 5 minutes
/auto-compact 120      # every 2 minutes
/auto-compact 600      # every 10 minutes
```

Interval is saved to `~/.claude/skills/auto-compact/config.json` and persists across wakes.

## What it does

- Sends a **push notification** to your phone/desktop and posts an in-chat nudge each interval
- **Silent during plan mode** — zero output, zero interruption while you're planning. Resumes nudging when plan mode ends
- Never runs `/compact` automatically — that's a runtime command only you can invoke
- Stop anytime: `stop auto-compact`

## Notes

- Interval range: 60s–3600s (clamped by runtime)
- Default: 300s (5 min)
