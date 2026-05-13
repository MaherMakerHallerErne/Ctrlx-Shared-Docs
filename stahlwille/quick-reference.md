---
title: Quick Reference
sidebar_label: Quick Reference
---

- Config path: Solutions → `he-ctrlx-app-commsw/appdata.json` (restart after edits)
- Output config: Solutions → `he-ctrlx-app-commsw/outputs.json` (buffering & HTTP posting)
- Data Layer nodes: `he/commsw/app/tool1/command`, `state`, `lastResult`
- Buffer status: `he/commsw/buffer/status` (queue, disk space, transmission stats)
- Enable pulse: each tightening requires a new enable rising edge
- Logs: Diagnostics → Logbook (filter by app source)
- Error codes: [Error Codes Reference](./error-codes.md) (negative values block Enable/Disable)
- Common fields: `pset`, torque limits `t_min/t_tgt/t_max`, angle (if required)
