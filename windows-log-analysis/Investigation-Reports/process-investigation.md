# Incident Report: Process Creation Monitoring (Notepad)

**Incident ID:** INC-2026-003  
**Event Source:** Sysmon Operational Log  
**Relevant Event ID:** 1 (Process Creation)  
**Severity:** Informational  
**Status:** Closed

## 1. Evidence Collected
- A **Sysmon Event ID 1** was generated.
- **Image:** `C:\Windows\System32\notepad.exe`
- **CommandLine:** `notepad.exe`
- **ParentImage:** `C:\Windows\System32\cmd.exe`
- **User:** Local User Account

## 2. Analysis
Sysmon Event ID 1 provides deep visibility into endpoint activity, recording the executable path, command-line arguments, and parent process. 

In this lab, I launched Notepad via the Command Prompt to simulate a basic process creation event. This is benign. However, a SOC analyst uses this exact data field to detect malicious execution. For example, if the Image was `powershell.exe` and the ParentImage was `outlook.exe`, that would indicate a phishing payload executing.

*Bonus Note: While reviewing Sysmon logs, a legitimate background process `pg_isready.exe` was observed running under the `NT SERVICE\Splunkd` user. This demonstrates the analyst's ability to filter out expected enterprise background noise from actual threats.*

## 3. Recommended Response (SOC Playbook)
1. **Verify the Hash:** Check the `Hashes` field (SHA256) of the process against VirusTotal to see if it is a known malicious file.
2. **Analyze the Parent:** Investigate the Parent Image. Is it normal for this parent to spawn this child? (e.g., `cmd.exe` spawning `notepad.exe` is normal; `winword.exe` spawning `cmd.exe` is suspicious).
3. **Check Command Line:** Look for unusual arguments, such as hidden windows (`-windowstyle hidden`) or downloading payloads.
