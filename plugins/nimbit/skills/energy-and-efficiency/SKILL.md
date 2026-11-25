---
name: energy-and-efficiency
description: 'Analyse energy use and efficiency in a Nimbit workspace: compare consumption across assets, areas and periods, find the biggest consumers and outliers, and optionally build a dashboard to keep watching it. Use when someone asks about energy, consumption, waste, efficiency or why a bill went up.'
---

# Energy and efficiency

Find where energy goes and where it is wasted. Be explicit about which measures you used and what they cover.

## Steps

1. Call `browse_hierarchy` to understand the structure (site, area, line) the user wants compared.
2. Call `find_things` for the energy-relevant assets in scope: meters, machines and aggregates. Use the thing type and labels, not the names alone.
3. Call `list_measures` (with `thingTypeId` or a sample `thingId`) for those thing types and choose the measures that really are energy, power, flow or efficiency. Note each measure's unit and whether it is a cumulative counter or an instantaneous value. Never add power readings as if they were energy.
4. Call `query_fleet` for one measure across all things of a type to rank them and find outliers. Compare against the fleet and against a comparable earlier period.
5. For the top consumers and outliers, call `query_timeseries` over the period to see when the use happens: base load at night, peaks, ramp-ups, and any sudden change.
6. If the workspace has a knowledge graph with context such as tariffs, asset documentation or maintenance history, call `query_knowledge_graph` to read it. Treat it as read-only context and say where it came from.

## Report

- **Where the energy goes**: the biggest consumers with value, unit and share of the total.
- **Outliers and waste candidates**: for example night or weekend base load, assets drifting up against their peers, and idle machines drawing power.
- **What changed**: against the comparison period, with the size of the change.
- **Suggestions**: ranked by likely saving, each tied to the evidence. Label estimates as estimates.
- **Limits**: measures you could not find, gaps in the data, and anything you assumed.

## Dashboard

If the user wants to keep watching this, offer a dashboard. `create_dashboard` creates a real dashboard in the workspace. Describe it first (title, which things or thing type, which measures) and wait for a yes before calling it. Share the link it returns.
