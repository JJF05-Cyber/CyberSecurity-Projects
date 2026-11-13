# Incident Report: PowerShell Script Block Execution

**Incident ID:** INC-2026-002  
**Event Source:** PowerShell Operational Log  
**Relevant Event ID:** 4104 (Script Block Logging)  
**Severity:** Informational  
**Status:** Closed

## 1. Evidence Collected
- A **Event ID 4104** was generated in the `Microsoft-Windows-PowerShell/Operational` log.
- The log captured the exact script block executed: `Get-Service`.
- The execution was performed by the local administrator/user.

## 2. Analysis
PowerShell is a powerful administrative tool, but it is heavily abused by attackers (Living off the Land - LotL) to execute malicious scripts without downloading external executables. 

The captured event shows a benign, legitimate administrative command (`Get-Service`). However, in a real SOC environment, an analyst must review 4104 events closely for **obfuscation**, **encoded commands** (e.g., `-EncodedCommand`), or attempts to bypass execution policies.

## 3. Recommended Response (SOC Playbook)
1. **Inspect the Script:** Review the `ScriptBlockText` in the Event ID 4104 log. Look for base64 encoding or suspicious cmdlets (e.g., `Invoke-WebRequest`, `IEX`).
2. **Check Parent Process:** Determine what launched PowerShell. If it was launched by `winword.exe` or `excel.exe`, it is a highly critical indicator of a macro-based malware attack.
3. **Containment:** If the script is determined to be malicious, isolate the endpoint from the network and initiate an incident response procedure.
