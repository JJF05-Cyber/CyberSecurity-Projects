# CyberSecurity-Projects

A collection of hands-on cybersecurity projects focused on Security Operations Center (SOC) workflows, threat detection, network forensics, and vulnerability assessment. Each project includes the objective, methodology, evidence, and a detailed analysis of findings.

## 📂 Project Index

| Project | Focus Area | Key Tools |
|---------|------------|-----------|
| [Phishing Email Triage](./phishing-triage) | Email Security & Threat Intel | VirusTotal, URLHaus, URLscan.io |
| [SSH Brute-Force Detection](./ssh-bruteforce) | Log Analysis & Attack Simulation | Kali Linux, Hydra, WSL Ubuntu |
| [Password Hash Analysis](./password-cracking) | Credential Attacks | John the Ripper, MD5, SHA-256 |
| [Network Traffic Analysis](./pcap-analysis) | Network Forensics & Malware Triage | Wireshark, Koi Stealer PCAP |
| [SIEM Alert Triage](./splunk-siem-triage) | SIEM Monitoring & Incident Triage | Splunk Enterprise, SPL |
| [Web Server Reconnaissance](./nikto-web-recon) | Vulnerability Assessment | Nikto, DVWA |
| [Nmap Scan & IDS Detection](./nmap-ids-detection) | Network Reconnaissance & Detection | Nmap, Suricata IDS |
| [Web Vulnerability Scanning](./zap-web-scan) | Web App Security (OWASP Top 10) | OWASP ZAP, DVWA |
| [Windows Log Analysis](./windows-log-analysis) | Endpoint Detection & Response (EDR) | Windows Event Viewer, Sysmon |

## 🎯 Focus Areas

| Category | Focus Areas | Tools Demonstrated |
|----------|-------------|-------------------|
| **Defensive (Blue Team)** | Threat detection, log analysis, incident triage | Splunk, Sysmon, Windows Event Logs, Suricata |
| **Network & Forensics** | PCAP analysis, C2 identification, IOC extraction | Wireshark, VirusTotal, URLHaus |
| **Offensive (Red Team)** | Reconnaissance, brute-force, vulnerability scanning | Kali Linux, Nmap, Hydra, Nikto, OWASP ZAP |
| **General Security** | Web app security, password analysis, OS hardening | John the Ripper, OWASP ZAP, DVWA |

## 💡 Skills Demonstrated
- **SIEM & Log Analysis:** Writing SPL queries, correlating Windows Event IDs (4624, 4625, 4104) and Linux `auth.log` files.
- **Network Forensics:** Analyzing PCAPs for C2 beaconing and identifying malicious infrastructure.
- **Threat Intelligence:** Extracting IOCs and pivoting across VirusTotal, URLHaus, and URLscan.io.
- **Vulnerability Assessment:** Conducting web server and application scans to identify SQLi, XSS, and missing security headers.
- **Incident Documentation:** Writing professional investigation reports and recommending SOC escalation playbooks.

## ⚠️ Disclaimer
All projects, attacks, and scans are performed in isolated, self-owned lab environments for educational purposes only. No unauthorized systems are targeted.
