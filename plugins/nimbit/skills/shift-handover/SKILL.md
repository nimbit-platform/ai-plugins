---
name: shift-handover
description: 'Write a shift handover note for a Nimbit workspace: incidents opened, resolved and still open during the shift, notable events, the state of key assets and what the next shift must watch. Optionally send a report if the operator asks. Use at the end of a shift or when someone asks for a handover.'
---

# Shift handover

Produce a note the next shift can act on without asking questions. Ask the user for the shift window if they did not give one; otherwise use the last 8 hours, and say so.

## Steps

1. Call `list_incidents` for the window: those opened during it, those resolved during it, and everything still open, including older ones that are carried over.
2. Call `list_events` for the window. Group repeats instead of listing each one, and call out anything unusual for this time of day.
3. Call `query_fleet` on the measures that describe the plant's health or output, to compare this shift with the previous one. Name the assets that differ most.
4. Call `find_things` for anything offline or not active, then `get_latest_values` for the assets the incidents and events point to, so the note states their current condition with units and the time of the last reading.

## The note

Use these sections, in this order, and leave out empty ones:

1. **Open incidents**: severity, thing, since when, who owns it, what has been tried, what is next.
2. **Resolved this shift**: what and how, in one line each.
3. **Watch next shift**: assets close to their limits, trends, recurring events, offline things.
4. **Changes and context**: anything that is not an incident but the next shift should know.

Write times in the workspace timezone. Do not guess an owner or a cause that the data does not show; write "unknown" instead.

## Sending a report

Only if the operator asks for a report to be sent, call `send_report`. Before calling it, say which report and to whom it goes, and wait for a clear yes. It queues an email to the report's subscribers, which you cannot undo. If the tool is not available, the reports toolset was not granted.
