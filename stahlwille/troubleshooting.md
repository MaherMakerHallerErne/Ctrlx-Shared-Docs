---
title: Troubleshooting
sidebar_label: Troubleshooting
---

| Symptom | Check | Action |
|---------|-------|--------|
| App fails to install | OS version / architecture | Ensure ctrlX OS V3.6.2; correct arm64/amd64 snap |
| Config not applied | Forgot restart | Restart after editing `appdata.json` |
| No connection | IP / Port wrong | Fix `IPAddress` / `IPPort` then restart |
| Wrong PSet behavior | PSet missing or malformed | Validate JSON & required fields |
| Enable blocked with error code | Error condition present | Check error code meaning in [Error Codes Reference](./error-codes.md) |
| Output buffer full | HTTP endpoint down | See [Output Curves Troubleshooting](./outputcurves.md#troubleshooting) |
| Log view broken | All columns enabled | Revert Logbook column layout |

Tips:
- Keep JSON minimal—remove unused fields.
- Change one PSet parameter at a time and restart.
- Check [Error Codes Reference](./error-codes.md) for operation error meanings and resolutions.
:::note Image Placeholder
`/img/user-guide/logbook-error.png` – Example error entry for missing PSet.
`/img/user-guide/config-diff.png` – Before/after config diff.
:::
