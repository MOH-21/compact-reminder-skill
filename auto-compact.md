# Auto-Compact Loop

Runs every 200s. Skips silently in plan mode. Nudges user to /compact when not in plan mode.

## On Each Wake

1. Check if current system context contains "Plan mode is active".
   - YES → plan mode active. Schedule next wake in 200s, output nothing, stop.
   - NO → continue.

2. Send PushNotification: "Context growing — check usage first, then /compact if needed"

3. Also output this in conversation (user may be watching):
   > **[auto-compact]** Context growing. Check usage (`/cost` or context % in status bar) — if it's still low, no action needed. Run `/compact` when it matters.

4. Schedule next wake in 200s with prompt `<<autonomous-loop-dynamic>>`.

## Notes

- 200s keeps prompt cache warm (under 5min TTL)
- Never compact automatically — /compact is a runtime command only the user can invoke
- In plan mode: zero output, zero interruption. Reschedule silently.
- Loop started by user typing `/auto-compact` or "start auto-compact"
- Loop stops when user types "stop auto-compact" or starts a new /loop
