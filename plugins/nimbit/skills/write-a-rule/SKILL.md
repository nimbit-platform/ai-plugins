---
name: write-a-rule
description: 'Draft an automation rule for a Nimbit workspace from a plain-language description: choose the thing type and measures, check existing rules, validate the conditions, replay them against stored telemetry, and save the result as a draft. Use when someone wants an alert or automation, for example "warn me when the pump runs hot for ten minutes".'
---

# Write a rule

Turn what the user wants into a rule that is correct, tested against real history and saved as a draft for a person to review. Never claim a rule is live.

## Steps

1. Restate the rule in one sentence (what condition, how long, for which things, what should happen) and confirm it with the user. Ask about anything vague: thresholds, durations, severity, which things.
2. Call `list_thing_types` and `get_thing_type` to find the thing type and its measures. Call `list_measures` to get the exact measure names, units and ranges. Use only measures that exist, in their own units.
3. Call `list_rules` to see whether a similar rule already exists. If one does, say so and ask whether to change it or add a new one.
4. Write the rule's conditions and call `validate_rule`. If it reports errors, fix them and validate again; do not continue until it passes. Use its `sample` parameter to check that a value which should trigger the rule does, and one that should not, does not.
5. Call `replay_rule` against stored telemetry for a handful of representative things over a period that includes a known problem and normal operation. Report how often it would have fired and when. A rule that fires constantly, or never, needs rethinking. Replay runs without executing any live action.
6. Show the final rule and the replay result, and ask the user whether to save it.

## Saving

`save_rule_draft` saves or updates a draft rule version for an existing `ruleId` only. It never enables, activates or publishes the rule, and the rule does nothing until a person reviews and publishes it in Nimbit. Say this when you save, and give the user the link or name so they can find the draft. Only call it after the user agrees to the final rule.

If `save_rule_draft` is not available, the automation write toolset was not granted. Give the user the validated rule text to enter in Nimbit instead.
