---
description: Investigate a Nimbit incident and, with your confirmation, acknowledge, assign or resolve it
argument-hint: '[incident id or description]'
---

Use the `investigate-incident` skill on this incident: $ARGUMENTS

If no incident was given, call `list_incidents` for the open ones and ask which to investigate.

Follow the skill: read it with `get_incident` and `list_events`, understand the thing with `get_thing`, `get_thing_type` and `list_measures`, check the data with `query_timeseries`, and add context with `ask_knowledge_graph`. Separate evidence from inference. Never call `acknowledge_incident`, `assign_incident` or `resolve_incident` without saying exactly what you will do and getting a clear yes first.
