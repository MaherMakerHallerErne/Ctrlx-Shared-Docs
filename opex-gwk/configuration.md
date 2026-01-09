---
title: Configuration (`appdata.json`)
sidebar_label: Configuration
---

## Location
Solutions file view:
```
…/solutions/configurations/appdata/content/edit/appdata.json?path=he-ctrlx-app-commgo%2F
```

## Required Fields
`IPAddress`, `IPPort`, `SerialNumber`, `Psets` (array of program definitions).

Supported PSet fields: `ParameterSetName`, `ParameterSetNumber`, `WorkpieceId`, `TorqueTarget` (and Min/Max/Threshold), `AngleTarget` (and Min/Max), `TimeMin`/`TimeMax`, `Method`, `Direction`, `Unit`, `CurveDataEnabled`.

## Example
```json
{
  "IPAddress": "10.10.2.177",
  "IPPort": 4545,
  "SerialNumber": "GP1010003",
  "Psets": [
    {
      "ParameterSetName": "Bolt M10",
      "ParameterSetNumber": 1,
      "WorkpieceId": "M10_BOLT",
      "ProgramNumber": "1",
      "TorqueTarget": 50,
      "TorqueMin": 45,
      "TorqueMax": 55,
      "TorqueThreshold": 5,
      "AngleTarget": 0,
      "AngleMin": 0,
      "AngleMax": 0,
      "BoltCenter": 0.5,
      "TimeMin": 100,
      "TimeMax": 5000,
      "Method": 1,
      "Direction": 1,
      "Unit": 1,
      "CurveDataEnabled": false
    }
  ]
}
```

![appdata.json editor view](./img/CommGo%20config%20app%20data.png)
