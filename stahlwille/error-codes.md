---
title: Error Codes Reference
sidebar_label: Error Codes
---

# Operation Error Codes

This document lists all error codes that can be returned by the ctrlX CORE CommSW application during Enable/Disable commands, HTTP output operations, and exposed through the DataLayer, REST API, and SignalR events.

## Error Code Table

| Code | Name | Message | Description |
|------|------|---------|-------------|
| **0** | `Success` | Operation completed successfully | Command executed successfully without errors. Positive confirmation. |
| **-1** | `NoLicense` | No valid license available | No valid license file found or license has expired. Tool cannot be enabled without a valid license. See [Licensing](./licensing.md) for license installation. |
| **-2** | `NoStorageAvailable` | No storage available (disk space or path issue) | Insufficient disk space for output buffering (available space < `MinStorageAvailable` threshold), or configured `StoragePath` is invalid/unwritable. See [Output Curves Troubleshooting](./outputcurves.md#storage-full-errors). |
| **-3** | `NotCompatible` | Protocol version not compatible | Tool protocol version mismatch. For Stahlwille 766, this typically indicates communication protocol incompatibility. Verify tool firmware version and communication settings. |
| **-4** | `MaxBufferLimitReached` | Maximum buffer limit reached for result files | Output buffer queue is full (`queued_items` = `MaxItemsInQueue`). Indicates HTTP endpoint is unavailable or processing slower than tightening rate. See [Output Curves Troubleshooting](./outputcurves.md#queue-full-errors). |
| **-5** | `PsetNotAvailable` | Requested parameter set not available or configuration is empty | The requested Pset (parameter set) number does not exist in `appdata.json`, or the Psets array is empty. Verify Pset configuration. See [Configuration](./configuration.md). |
| **-6** | `ToolNotConnected` | Tool is not connected | Tool is not connected via TCP/IP. Check tool is powered on, WiFi is connected (for Stahlwille 766), and `IPAddress`/`IPPort` in `appdata.json` are correct. See [Troubleshooting](./troubleshooting.md). |
| **-7** | `ToolAlreadyEnabled` | Tool is already enabled | Attempted to enable tool when it is already in enabled state. Disable tool first before enabling with different Pset. |

## Error Code Behavior

### Negative Values Block Commands

**All negative error codes (-1 through -7) indicate conditions that block Enable/Disable commands**. When these errors are present:

- **Enable command**: Returns the error code and refuses to enable the tool
- **Disable command**: May return error code depending on the specific condition
- **Tool remains in safe state**: Tool will not perform tightening operations until error is resolved

### Where Error Codes Appear

Error codes are exposed through multiple interfaces to support different use cases:

#### 1. REST API (HTTP Endpoints)

**Enable endpoint**: `POST /he-ctrlx-app-commsw/api/v1/tool/enable?iPSet=2`

**Success response** (HTTP 200):
```json
{
  "success": true,
  "errorCode": 0,
  "message": "Operation completed successfully"
}
```

**Error response** (HTTP 400):
```json
{
  "success": false,
  "errorCode": -6,
  "message": "Tool is not connected"
}
```

#### 2. SignalR Real-Time Events

**Event**: `CommandRejected`

**Payload structure**:
```json
{
  "errorCode": -5,
  "errorMessage": "Requested parameter set not available or configuration is empty",
  "command": "Enable",
  "timestamp": "2026-01-09T14:32:15Z"
}
```

**Usage**: Frontend subscribes to `CommandRejected` event to display error notifications to operators in real-time when Enable/Disable commands fail.

#### 3. DataLayer (PLC Integration)

**Tool state node**: `he/commsw/app/tool1/state`

**Structure** (FlatBuffers):
```
{
  "connected": false,
  "enabled": false,
  "pset": 0,
  "error_code": -6,
  "error_message": "Tool is not connected"
}
```

**Usage**: PLC reads `error_code` field from tool state to determine if tool is operational. Negative values trigger production line error handling logic.

## Error Resolution Guide

### -1 NoLicense

**Problem**: Application cannot find valid license file or license has expired.

**Resolution**:
1. Obtain valid license file from vendor
2. Place license file in Solutions configuration directory alongside `appdata.json`
3. Restart application
4. Verify license is loaded by checking application logs

**See**: [Licensing Guide](./licensing.md) for detailed installation instructions.

### -2 NoStorageAvailable

**Problem**: Disk space below threshold or storage path invalid.

**Resolution**:
1. Check available disk space: Review `available_disk_space_mb` in `he/commsw/buffer/status`
2. Free disk space if low (delete old files, logs, or unused snaps)
3. Reduce `MinStorageAvailable` threshold in `outputs.json` if too conservative
4. Configure external storage (SD card/USB) via `StoragePath`
5. If output not needed, set `EnableOutput: false` in `outputs.json`

**See**: [Output Curves - Storage Full Errors](./outputcurves.md#storage-full-errors)

### -3 NotCompatible

**Problem**: Protocol version mismatch between application and tool.

**Resolution**:
1. Verify tool firmware version is compatible with application version
2. Check tool communication settings (baud rate, parity for serial connections)
3. For Stahlwille 766: Ensure tool is in proper operating mode
4. Review application logs for detailed protocol negotiation errors
5. Contact vendor support if protocol incompatibility persists

### -4 MaxBufferLimitReached

**Problem**: Output buffer queue is full, HTTP endpoint not processing results fast enough.

**Resolution**:
1. Check HTTP endpoint is running and accessible
2. Test endpoint connectivity: `curl -X POST http://127.0.0.1:8889/x -d '{"test": true}'`
3. Review `he/commsw/buffer/status`:
   - Check `queued_items` value (should be < `MaxItemsInQueue`)
   - Check `total_transmitted` is incrementing (indicates endpoint responding)
   - Check `total_failed_attempts` (high value indicates HTTP errors)
4. Temporarily increase `MaxItemsInQueue` in `outputs.json` to accommodate backlog
5. If output not critical, set `EnableOutput: false` to disable buffering

**See**: [Output Curves - Queue Full Errors](./outputcurves.md#queue-full-errors)

### -5 PsetNotAvailable

**Problem**: Requested parameter set (Pset) does not exist in configuration.

**Resolution**:
1. Open `appdata.json` in Solutions configuration directory
2. Verify Pset with requested number exists in `Psets` array
3. Check Pset numbering is sequential (1, 2, 3, ...)
4. Validate JSON syntax (use JSON validator if needed)
5. Restart application after fixing configuration

**Example valid Psets array**:
```json
{
  "Psets": [
    { "iPSet": 1, "Type": "JNT", "Tnom": 20.0, "Tmin": 18.0, "Tmax": 22.0 },
    { "iPSet": 2, "Type": "JNA", "Angle": 90, "AngleMin": 85, "AngleMax": 95 }
  ]
}
```

**See**: [Configuration Guide](./configuration.md)

### -6 ToolNotConnected

**Problem**: TCP/IP connection to tool is not established.

**Resolution**:
1. Verify tool is powered on and operational
2. For Stahlwille 766 WiFi:
   - Check WiFi connection indicator on tool
   - Verify ctrlX CORE is on same network as tool
   - Ping tool IP address: `ping 10.10.2.126`
3. Check `appdata.json` configuration:
   - `IPAddress`: Must match tool's IP address
   - `IPPort`: Must match tool's TCP port (typically 4002 for Stahlwille)
4. Restart application after fixing configuration
5. Check application logs for TCP connection error details

**See**: [Troubleshooting Connection Issues](./troubleshooting.md)

### -7 ToolAlreadyEnabled

**Problem**: Attempted to enable tool that is already enabled.

**Resolution**:
1. Send Disable command first
2. Wait for tool to enter disabled state (check `state` node or REST API)
3. Send Enable command with desired Pset number

**Note**: This is typically a programming error in PLC logic or frontend application. Normal operation should track enabled state and not attempt redundant Enable commands.

## Programming Examples

### Frontend (TypeScript/Angular)

```typescript
import { OperationErrorCode, getOperationErrorMessage } from './models/operation-error-code';

// Handle Enable command response
toolService.enableTool(psetNumber).subscribe({
  next: (response) => {
    if (response.errorCode === OperationErrorCode.Success) {
      console.log('Tool enabled successfully');
    } else {
      const message = getOperationErrorMessage(response.errorCode);
      alert(`Enable failed: ${message}`);
    }
  },
  error: (err) => {
    console.error('HTTP error:', err);
  }
});

// Handle SignalR CommandRejected event
signalRService.commandRejected$.subscribe((event) => {
  const message = getOperationErrorMessage(event.errorCode);
  this.showNotification(`Command ${event.command} rejected: ${message}`, 'error');
});
```

### PLC (Structured Text)

```pascal
PROGRAM ToolController
VAR
    toolState : ToolState;           // Read from he/commsw/app/tool1/state
    enableCommand : BOOL;
    lastErrorCode : INT;
    errorMessage : STRING[255];
END_VAR

// Read tool state from DataLayer
toolState := DataLayer.Read('he/commsw/app/tool1/state');

// Check for errors before enabling
IF enableCommand AND NOT toolState.enabled THEN
    IF toolState.error_code < 0 THEN
        // Negative error code - cannot enable
        lastErrorCode := toolState.error_code;
        errorMessage := toolState.error_message;
        
        CASE toolState.error_code OF
            -1: (* NoLicense - Critical, stop production *)
                ProductionAlarm := TRUE;
                AlarmMessage := 'License invalid';
            -2: (* NoStorageAvailable - Warning, may continue if output not critical *)
                ProductionWarning := TRUE;
            -6: (* ToolNotConnected - Common, retry connection *)
                RetryConnection := TRUE;
        END_CASE;
    ELSE
        // No errors, send Enable command
        DataLayer.Write('he/commsw/app/tool1/command', enableCommand);
    END_IF;
END_IF;
```

## Related Documentation

- [Configuration Guide](./configuration.md) - Configuring Psets and connection settings
- [Output Curves Export & Posting](./outputcurves.md) - Buffering errors -2 and -4
- [Licensing Guide](./licensing.md) - Resolving error -1
- [Troubleshooting](./troubleshooting.md) - General troubleshooting tips
- [Data Layer Control](./data-layer.md) - PLC integration and error handling
