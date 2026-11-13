# Project : Windows Log Analysis & Threat Detection

## Objective
Configure Windows security auditing and Sysmon to collect endpoint telemetry, generate security events, and investigate them using Windows Event Viewer.

## Lab Setup
- **Environment**: Windows Host
- **Tools**: Event Viewer, Sysmon, PowerShell, Windows Command Prompt

## Methodology
1. Enabled advanced auditing via `auditpol` for Logon, Process Creation, and Account Management.
2. Installed Microsoft Sysmon to capture detailed endpoint telemetry (process creation, network connections).
3. Generated benign security events (failed logins, PowerShell execution, process creation).
4. Investigated the events using Windows Event Viewer, focusing on critical Event IDs.

## Findings & Investigation

### 1. Authentication Activity (Event ID 4625 & 4624)
- **Action**: Simulated failed logins followed by a successful login.
- **Event Viewer**: Navigated to `Windows Logs > Security`.
- **Investigation**: Identified multiple Event ID `4625` (Failed login) followed by Event ID `4624` (Successful login). As an analyst, this sequence requires investigation to rule out an account lockout or brute-force attempt.

### 2. Process Creation (Sysmon Event ID 1)
- **Action**: Opened `notepad.exe`.
- **Event Viewer**: Navigated to `Applications and Services Logs > Microsoft > Windows > Sysmon > Operational`.
- **Investigation**: Sysmon Event ID `1` provided detailed telemetry including the process Image path, the user account, and the parent process. This is critical for identifying malicious process execution (e.g., a Word document spawning PowerShell).

### 3. PowerShell Activity (Event ID 4104)
- **Action**: Executed a benign PowerShell command.
- **Event Viewer**: Navigated to `PowerShell > Operational`.
- **Investigation**: Event ID `4104` (Script Block Logging) captured the actual code executed. SOC analysts use this to detect obfuscated or malicious scripts running in the environment.

## SOC Relevance
Endpoint detection and log analysis are foundational Tier 1 SOC skills. This project demonstrates:
- Ability to configure Windows auditing and deploy Sysmon.
- Proficiency in navigating Windows Event Viewer to find specific Event IDs (4624, 4625, 4104, Sysmon 1).
- The ability to reconstruct a timeline of user and system activity for incident investigation.

## Tools Used
Windows Event Viewer, Sysmon, PowerShell, `auditpol`.
