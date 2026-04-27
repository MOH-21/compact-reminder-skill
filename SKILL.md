---
name: auto-compact
description: Reminds you to /compact during long sessions. Loops at a user-defined interval (default 300s), skips silently in plan mode, sends push notification + in-chat nudge when context is growing.
---

# Auto-Compact Loop

Runs at a configurable interval. Skips silently in plan mode. Nudges user to /compact when not in plan mode.

## Config file

Interval is persisted at `~/.claude/skills/auto-compact/config.json`:
```json
{"interval": 300}
```
Written on start, read on every wake. This is the only way to carry state across `ScheduleWakeup` wakes — the wakeup mechanism only carries a prompt string, not arbitrary data.

## On Start (skill invoked by user)

1. Read the skill args:
   - If a number is provided (e.g. `/auto-compact 120` or `/auto-compact 600`), treat it as the interval in seconds.
   - If no arg, default to 300s.
   - Clamp to [60, 3600].
2. Write `{"interval": N}` to `~/.claude/skills/auto-compact/config.json` using the Write tool.
3. Confirm to the user: "Auto-compact started. Nudging every {N}s. Run `stop auto-compact` to stop."
4. Schedule first wake in {N}s with prompt `<<autonomous-loop-dynamic>>`.

## On Each Wake

1. Check if current system context contains "Plan mode is active".
   - YES → plan mode active. Read config for interval (default 300s if missing). Schedule next wake silently, output nothing, stop.
   - NO → continue.

2. Read `~/.claude/skills/auto-compact/config.json` using the Read tool. Use `interval` value. If file missing or unreadable, default to 300s.

3. Send PushNotification: "Context growing — run /compact to stay efficient"

4. Output in conversation:
   > **[auto-compact]** Context growing. Run `/compact` to compact now.

5. Schedule next wake in {N}s with prompt `<<autonomous-loop-dynamic>>`.

## Notes

- Default interval: 300s (5 min)
- User can pass seconds as arg: `/auto-compact 120` = nudge every 2 min
- Interval clamped to [60, 3600] by ScheduleWakeup runtime
- Never compact automatically — /compact is a runtime command only the user can invoke
- In plan mode: zero output, zero interruption. Reschedule silently.
- Loop stops when user types "stop auto-compact" or starts a new /loop
