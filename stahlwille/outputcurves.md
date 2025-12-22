# Output Curves Export & Posting

This document explains how tightening curve data is automatically exported as JSON and posted to the sys3xx gateway via HTTP request, configured through the `outputs.json` file.

## Configuration File: outputs.json

The `outputs.json` file controls the export and posting behavior. It is located alongside `appdata.json` in the application configuration directory.

Example `outputs.json`:
```json
{
  "OutputHttpURL": "http://127.0.0.1:8889/x",
  "TempStoragePath": ""
}
```

- `OutputHttpURL`: The HTTP endpoint URL where the JSON data will be posted. This should point to your sys3xx gateway.
- `TempStoragePath`: Reserved for future use (local storage).

## Automatic HTTP Posting to sys3xx

When a tightening cycle completes, the application automatically:

1. Records the tightening curve data (torque, angle, time points).
2. Generates a JSON payload containing the cycle information and curve data.
3. Posts the JSON to the URL specified in `OutputHttpURL` using an HTTP POST request with `Content-Type: application/json`.

The posting is asynchronous and fire-and-forget - it logs success or failure but does not block the tightening process.

### Integration with sys3xx Gateway

To integrate with sys3xx:

1. Ensure sys3xx gateway is running and listening on the configured URL.
2. Set `OutputHttpURL` in `outputs.json` to point to the gateway's endpoint (e.g., `http://127.0.0.1:8889/x`).
3. The application will automatically send tightening data after each cycle.

### Disabling Export

To disable automatic posting, remove or leave `OutputHttpURL` empty in `outputs.json`.

## Troubleshooting

- **No data received**: Check that `OutputHttpURL` is correctly set and the sys3xx gateway is accessible.
- **Posting errors**: Review application logs for HTTP error codes or connection issues.
