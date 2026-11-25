---
name: plant-health-summary
description: 'Summarise the health of a Nimbit workspace or one part of it (a site, line or area): what is online, what is out of range, which incidents are open and what happened recently. Use when someone asks how the plant is doing, for a status overview, or at the start of a shift.'
---

# Plant health summary

Give a short, honest picture of how the workspace is doing right now. Lead with what needs attention; do not list everything that is fine.

## Steps

1. Call `get_workspace` for the workspace name, timezone and which toolsets are granted. State all times in that timezone.
2. Decide the scope. If the user named a site, line or area, call `browse_hierarchy` and find its node. Otherwise summarise the whole workspace, starting from the top levels of `browse_hierarchy`.
3. Call `find_things` for the things in scope. Note how many are offline or in a non-active lifecycle state.
4. For the things that matter (the offline ones, and the key assets of each thing type), call `get_latest_values`. Flag stale readings: a value that has not updated for much longer than the thing's usual interval is a finding, not a number.
5. For a fleet of one thing type, call `query_fleet` on the measure that best describes its health, and name outliers against the fleet. Pick the measure from the thing type's measures; do not guess a name.
6. Call `list_incidents` for open incidents in scope, then `list_events` for the last 24 hours (or the period the user gave) to see what is new and what keeps repeating.

## Report

- **Verdict** in one line: healthy, needs attention or critical, with the reason.
- **Needs attention**: open incidents by severity, offline things, readings out of range or stale. Give each thing's name, the value with its unit and when it was reported.
- **Trends worth knowing**: repeating events, fleet outliers.
- **Not checked**: anything you could not read, for example because a toolset was not granted. Say so instead of implying it is fine.

Offer to continue with the `investigate-incident` skill for any incident, and link to the thing or incident in Nimbit where a tool result gives a link.

This skill only reads data. It changes nothing.
