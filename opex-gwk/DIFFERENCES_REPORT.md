# Documentation Differences Report: Stahlwille vs. OPEX-GWK

This report tracks the automated migration of documentation from the Stahlwille tool integration to the OPEX-GWK integration.

## Auto-Resolved Differences

| Feature | Stahlwille (Source) | OPEX-GWK (Target) | Notes |
| :--- | :--- | :--- | :--- |
| **Application ID** | `he-ctrlx-app-commsw` | `he-ctrlx-app-commgo` | Full replacement in paths and commands. |
| **Friendly Name** | "Stahlwille Communication App" | "GWK Operator App" | Updated in installation/UI references. |
| **Default Tool IP** | `10.10.2.126` (WiFi) | `10.10.2.177` (Ethernet) | Updated in configuration examples. |
| **Default Tool Port** | `4002` | `4545` | Updated in configuration/troubleshooting. |
| **Data Layer Root** | `he/commsw` | `he/commgo` | Updated in `data-layer.md` and `outputcurves.md`. |
| **PSet Config** | `pset`, `name`, `type`, `t_tgt`... | `ParameterSetName`, `TorqueTarget`... | Structure updated in `configuration.md` to match CommGO `appdata.json`. |
| **Output Buffer Path** | `he/commsw/buffer` | `he/commgo/buffer` | Updated in Output Curves docs. |

## Manual Action Items

The following items require manual review or asset creation:

- [ ] **UI Screenshots**: `ui-widget.md` refers to `Widget_ShowCase.gif` which shows Stahlwille UI. Needs update for GWK branding/layout.
- [ ] **Architecture Diagram**: `user-guide.md` refers to `overview-architecture.png`. Verify if data flow diagram needs protocol-specific updates.
- [ ] **Licensing Images**: `licensing.md` images are placeholders. Verify if the licensing screen (License Generator) looks identical.
- [ ] **Logbook Images**: `troubleshooting.md` refers to `logbook-error.png`.
- [ ] **Cheatsheet**: `quick-reference.md` link to `cheatsheet.png` needs validation.
- [ ] **Overview**: `overview.md` was overwritten/ignored in favor of migrated `user-guide.md` structure. Verify if `overview.md` in source had unique content (it was "Coming Soon").

## Compliance Checklist

| Document | Status | Notes |
| :--- | :--- | :--- |
| `installation.md` | ✅ Migrated | App IDs and images updated. |
| `configuration.md` | ✅ Migrated | JSON structure completely updated for CommGO. |
| `user-guide.md` | ✅ Migrated | Main entry point updated. |
| `troubleshooting.md` | ✅ Migrated | Error codes and paths updated. |
| `data-layer.md` | ✅ Migrated | Node paths updated to `he/commgo`. |
| `ui-widget.md` | ✅ Migrated | Text updated, image marked TODO. |
| `licensing.md` | ✅ Migrated | Content generic, images marked TODO. |
| `applying-changes.md` | ✅ Migrated | Restart image updated. |
| `error-codes.md` | ✅ Migrated | Full error code table migrated with updated context. |
| `outputcurves.md` | ✅ Migrated | Deep technical reference updated for CommGO paths. |
| `quick-reference.md` | ✅ Migrated | Summary updated. |
