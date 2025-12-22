---
title: Data Layer Control
sidebar_label: Data Layer
---

## Main Node
`he/commsw/app/tool1` exposing `command`, `state`, `lastResult`.

Write `command` to set `pset` and pulse `enable` (rising edge loads PSet; falling edge aborts active tightening). Continuous enable not supported.

Errors (missing PSet, transfer failures, unlicensed software) reported through `state`.
![Datalayer Type List](./img/Datalayer%20list.png)

![Data Layer Tool1](./img/Data%20layer%20tool1.png)

![Data Layer Disable](./img/Data%20Layer%20Disable.png)

### Summary
- `command`: write (pset + enable edge)
- `state`: read (connection/operational)
- `lastResult`: read (torque/angle/result JSON)

