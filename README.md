# ASIM NetworkSession Parser — Ubiquiti UniFi Network

Advanced SIEM Information Model (ASIM) `NetworkSession` schema parser for **Ubiquiti UniFi Network** (UDM Pro) CEF-over-syslog logs, for use in Microsoft Sentinel / Log Analytics.

## Source data

- Table: `Ubiquiti_CL` (raw CEF syslog stored in the `Message` column)
- Vendor/Product: Ubiquiti / UniFi Network (UDM Pro)

## Coverage

Normalizes 11 UniFi CEF event classes as network connectivity sessions:

- **201** — Threat Detected
- **203** — Blocked by Firewall
- **400–404** — WiFi/Wired client connect, disconnect, and roam events
- **520–521** — VPN connect/disconnect
- **522–523** — Teleport connect/disconnect

## Files

| File | Description |
|---|---|
| `ASimNetworkSessionUbiquitiUniFiNetwork.kql` | Parameterless ASIM parser (built on top of the raw table) |
| `vimNetworkSessionUbiquitiUniFiNetwork.kql` | Parameterized ASIM parser with standard NetworkSession filtering parameters (`starttime`, `endtime`, `srcipaddr_has_any_prefix`, `dstipaddr_has_any_prefix`, `ipaddr_has_any_prefix`, `dstportnumber`, `hostname_has_any`, `dvcaction`, `eventresult`, `disabled`, `pack`) |
| `deploy_UbiquitiUniFiNetworkSession.json` | ARM template to deploy both saved-search functions to a Log Analytics workspace |

## Validation status

- ASIM Schema validation: ✅ Passed (zero errors)
- ASIM Data validation: ✅ Passed (zero errors)
- Filter parameter validation: ✅ Passed (one accepted exception — `eventresult` is always `"Success"` since UniFi CEF logs carry no failure signal for these event types)

## Deployment

```powershell
az deployment group create \
  --resource-group <resourceGroup> \
  --template-file deploy_UbiquitiUniFiNetworkSession.json \
  --parameters Workspace=<workspaceName> WorkspaceRegion=<region>
```

## Schema version

ASIM `NetworkSession` schema version `0.2.7`.
