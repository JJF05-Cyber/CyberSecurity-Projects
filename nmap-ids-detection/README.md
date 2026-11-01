# Project : Nmap Scan Detection with Suricata IDS

## Objective
Simulate a network reconnaissance scan with Nmap and detect it using Suricata IDS, demonstrating the ability to correlate attacker activity with defender alerts.

## Lab Setup
- **Platform**: Kali Linux
- **Scanner**: Nmap
- **IDS**: Suricata (monitoring loopback interface)
- **Target**: Localhost with a Python HTTP server running on port 8000

## Methodology
1. Configured Suricata to monitor the loopback interface (`lo`) with `HOME_NET` set to `127.0.0.1/32`.
2. Started a Python HTTP server on port 8000 as a scan target.
3. Ran an aggressive Nmap scan (`-sS -sV -T4`) against localhost.
4. Checked Suricata's `fast.log` for scan detection alerts.

## Findings

### Nmap Scan
Command:

    sudo nmap -sS -sV -T4 127.0.0.1

Result: Nmap identified port 8000/tcp open (Python HTTP server).

### IDS Detection
Suricata triggered the following alert from the Emerging Threats ruleset:

    11/01/2026-11:17:55.263859  [**] [1:2024364:5] ET SCAN Possible Nmap User-Agent Observed [**] [Classification: Web Application Attack] [Priority: 1] {TCP} 127.0.0.1:35386 -> 127.0.0.1:8000

The rule matches Nmap's default User-Agent string during service detection, which is a reliable indicator of reconnaissance activity.

### Additional Alerts
`ET INFO Python SimpleHTTP ServerBanner` also fired, confirming the HTTP service was probed.

## SOC Relevance
Network scanning is a precursor to exploitation. SOC analysts must recognize scan alerts and correlate them with subsequent attack activity. This project demonstrates:
- Understanding of how Nmap scans appear in IDS logs
- Ability to configure Suricata to monitor a specific interface
- Correlation of offensive tooling with defensive detection

## Tools Used
Kali Linux, Nmap, Suricata, Python HTTP server
