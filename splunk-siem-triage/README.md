# Project : SIEM Alert Triage Simulation (Splunk)

## Objective
Ingest Linux authentication logs into Splunk, write SPL queries to detect a brute-force attack, and perform incident triage to identify post-exploitation activity.

## Lab Setup
- **Platform**: Windows Host
- **Tool**: Splunk Enterprise (Free License)
- **Log Source**: Linux `auth.log` containing a simulated SSH brute-force attack.

## Methodology
1. Ingested the `auth.log` file into Splunk using the `linux_secure` sourcetype.
2. Wrote SPL queries to aggregate failed login attempts by source IP.
3. Correlated failed attempts with successful logins to identify a breach.
4. Analyzed subsequent logs to track post-exploitation activity (persistence, privilege escalation).

## Findings

### 1. Brute-Force Detection
SPL query used:

    index=main sourcetype=linux_secure "Failed password"
    | rex "from (?<src_ip>\d+\.\d+\.\d+\.\d+)"
    | stats count by src_ip
    | sort -count

**Result**: Source IP `65.2.161.68` generated **96 failed login attempts** against various users (`root`, `admin`, `backup`, `svc_account`), indicating an automated SSH brute-force attack.

### 2. Successful Compromise
SPL query used:

    index=main sourcetype=linux_secure "Accepted password" "65.2.161.68"

**Result**: Despite the failures, the attacker successfully authenticated as `root` at 06:31:40 and 06:32:44.

### 3. Post-Exploitation Activity
Analysis of logs following the successful login revealed a multi-stage attack:
- **Persistence**: The attacker created a new local user named `cyberjunkie` (06:34:18).
- **Privilege Escalation**: The new user was added to the `sudo` group (06:35:15).
- **Credential Access**: The attacker used `sudo` to read `/etc/shadow` (06:37:57).
- **Tool Download**: The attacker used `sudo` to download a privilege escalation script (`linper.sh`) from GitHub (06:39:38).

## SOC Relevance
SIEM alert triage is the core daily task of a Tier 1 SOC analyst. This project demonstrates:
- Ability to ingest and search logs in Splunk.
- SPL query writing for brute-force detection.
- Correlation of failed and successful authentication events to identify a breach.
- Incident documentation tracking the full attack lifecycle (Initial Access → Persistence → Privilege Escalation → Credential Access).

## Tools Used
Splunk Enterprise, SPL (Search Processing Language)
