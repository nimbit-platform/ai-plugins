---
name: onboard-a-device
description: 'Help bring a new device into a Nimbit workspace: pick or check its thing type, confirm the device exists, read its ingest errors, send a test reading and optionally start a simulation. Use when someone connects a new device or sensor, or a device is not showing data.'
---

# Onboard a device

Get a device from "connected" to "sending data the workspace understands". Work from what the platform reports, not from assumptions about the device.

## Steps

1. Call `list_thing_types` to see what exists, and call `get_thing_type` for the likely match. Check its measures, units and data types against what the device sends. If nothing fits, say what is missing; creating or changing thing types is done in Nimbit, not here.
2. Call `find_things` to see whether the device already exists (search by name, identifier or thing type). Then call `get_thing` to see its type, connectivity and latest values.
3. If the device is not sending, or sends bad data, call `list_ingest_errors`. Explain each error in plain words and map it to a fix: an unknown measure, a wrong data type, a unit or range problem, or a payload shape the thing type does not expect.
4. To prove the path end to end, call `send_test_reading` (for a thing marked as a test thing — `"test"` in `searchLabels` or `customProperties.test = true` — or while a `start_simulation` run is active) with values that match the thing type's measures and units. Then call `get_thing` again to confirm the value arrived, and call `list_ingest_errors` again to check nothing was rejected.
5. If the user wants to see the device in dashboards before real data arrives, offer `start_simulation`.

## Before you write

`send_test_reading` and `start_simulation` create data in the workspace, and a test reading can trigger rules, events and incidents. Before calling either:

- Say which thing it is for, which values you will send, or how long and at what rate the simulation runs.
- Make sure the user knows the readings are test data and may be seen by others and by live rules.
- Wait for a clear yes. Use the smallest reading that proves the point.

If these tools are not available, the integration toolset was not granted.

## Report

State what is now true: thing type, whether the device exists, whether data arrives, which errors remain, and the next step for the user.
