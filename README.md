# auto-compact

Claude Code skill that reminds you to `/compact` during long sessions — skips silently in plan mode.

## Install

```bash
npx skills add MOH-21/auto-compact-skill@auto-compact
```

## Usage

```
/auto-compact
```

Fires every 200s. Sends a push notification + in-chat nudge when context is growing. Does nothing during plan mode so it never interrupts your planning flow.

## Notes

- 200s interval keeps prompt cache warm (under 5min TTL)
- Cannot auto-run `/compact` — that's a runtime command only you can invoke
- Stop with: `stop auto-compact`
