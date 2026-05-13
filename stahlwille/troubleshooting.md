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
