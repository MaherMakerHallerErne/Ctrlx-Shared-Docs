---
title: Data Layer Control
sidebar_label: Data Layer
---

## Main Node
`he/commgo/app/tool1` exposing `command`, `state`, `lastResult`.

Write `command` to set `pset` and pulse `enable` (rising edge loads PSet; falling edge aborts active tightening). Continuous enable supported in OPEX protocol (Command: `61`, Param: `1`=Enable).

Errors (missing PSet, transfer failures, unlicensed software) reported through `state` node with `error_code` field. Negative error codes block Enable/Disable commands. See [Error Codes Reference](./error-codes.md) for complete list of error codes and resolutions.

### Buffer Status Monitoring

`he/commgo/buffer/status` - Output buffer statistics for monitoring HTTP transmission health:

- `queued_items` - Number of results awaiting transmission
- `total_transmitted` - Lifetime successful transmissions
- `available_disk_space_mb` - Available storage space
- `is_disk_space_critical` - True when storage critically low

See [Output Curves - DataLayer Integration](./outputcurves.md#datalayer-integration) for complete field descriptions and PLC monitoring patterns.
![Datalayer Type List](./img/CommGo%20datalayer%201.png)

![Data Layer Tool1](./img/CommGo%20datalayer%201.png)

![Data Layer Disable](./img/CommGo%20datalayer%201.png)

### Summary
- `command`: write (pset + enable/disable)
- `state`: read (connection/operational)
- `lastResult`: read (torque/angle/result JSON)
