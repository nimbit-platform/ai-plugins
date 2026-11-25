---
name: investigate-incident
description: 'Investigate a Nimbit incident from symptom to likely cause using its events, the affected thing, its measures and recent telemetry, and the workspace knowledge graph. Then, only after confirmation, acknowledge, assign or resolve it. Use when someone asks why an incident happened, what to do about an alert, or to work an incident.'
---

# Investigate an incident

Find out what happened and why, then help the operator act. Separate what the data shows from what you infer.

## Steps

1. Get the incident with `get_incident`. If the user gave a description rather than an id, find it with `list_incidents` first and confirm which one they mean.
2. Call `list_events` for the incident's thing and the hours before it opened, to see the sequence: what fired first, what followed, whether it has happened before.
3. Call `get_thing` for the affected thing: type, location, labels, connectivity and latest values. Call `get_thing_type` for its declared measures, thresholds and commands, so you read values against the right limits.
4. Call `list_measures` to pick the measures related to the symptom, then `query_timeseries` for those measures from well before the incident to now. Look for the change that preceded it: a drift, a step, a flatline or a gap. Compare with a normal period.
5. Call `ask_knowledge_graph` for context a person would want: related assets, documents, past incidents and procedures. Quote what it returns with its sources and do not present its answer as measured data.

## Report

- **What happened**: one or two sentences, with times in the workspace timezone.
- **Evidence**: the events, values (with units) and chart findings that support it.
- **Likely cause**: marked as an inference, with how confident you are and what would confirm it.
- **Suggested next step**: a concrete action for the operator.

## Acting on the incident

`acknowledge_incident`, `assign_incident` and `resolve_incident` change the incident and are visible to the whole team. Before each one:

- Say exactly what you are about to do: which incident, which action, and for assignment who gets it, for resolution which reason and note.
- Wait for the user to say yes. Never act because the evidence looks clear.
- Do one action at a time, and report the new state afterwards.

For `resolve_incident`, use only a resolution reason the workspace offers. If you are not sure which fits, ask the user.

If these tools are not available, the operations toolset was not granted. Tell the user to reconnect and grant it, or to act in Nimbit.
