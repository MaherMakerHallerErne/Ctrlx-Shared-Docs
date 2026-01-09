---
title: Data Layer Control
sidebar_label: Data Layer
---

## Main Node
`he/commsw/app/tool1` exposing `command`, `state`, `lastResult`.

Write `command` to set `pset` and pulse `enable` (rising edge loads PSet; falling edge aborts active tightening). Continuous enable not supported.

Errors (missing PSet, transfer failures, unlicensed software) reported through `state` node with `error_code` field. Negative error codes block Enable/Disable commands. See [Error Codes Reference](./error-codes.md) for complete list of error codes and resolutions.

### Buffer Status Monitoring

`he/commsw/buffer/status` - Output buffer statistics for monitoring HTTP transmission health:

- `queued_items` - Number of results awaiting transmission
- `total_transmitted` - Lifetime successful transmissions
- `available_disk_space_mb` - Available storage space
- `is_disk_space_critical` - True when storage critically low

See [Output Curves - DataLayer Integration](./outputcurves.md#datalayer-integration) for complete field descriptions and PLC monitoring patterns.
![Datalayer Type List](./img/Datalayer%20list.png)

![Data Layer Tool1](./img/Data%20layer%20tool1.png)

![Data Layer Disable](./img/Data%20Layer%20Disable.png)

### Summary
- `command`: write (pset + enable edge)
- `state`: read (connection/operational)
- `lastResult`: read (torque/angle/result JSON)

