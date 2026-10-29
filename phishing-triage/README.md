# Project 1: Phishing Email Triage & IOC Analysis

## Objective
Analyze a simulated phishing email, extract indicators of compromise (IOCs), and pivot across threat intelligence platforms to determine the nature and severity of the threat.

## Sample Source
CyberDefenders — PhishStrike Challenge (.eml file)

## Methodology
1. Opened the `.eml` file in a text editor (never executed it) and inspected raw email headers.
2. Checked `Authentication-Results` for SPF/DKIM failures.
3. Extracted the embedded URL and sender IP.
4. Investigated the URL on VirusTotal and URLHaus.
5. Documented findings and IOCs.

## Findings

### Email Header Analysis
- **Authentication-Results**: `spf=softfail`, `dkim=fail` → sender spoofing confirmed.
- **Sender IP**: `18.208.22.104`
- **Return-Path** differs from the displayed "From" address → common phishing indicator.

### URL Analysis
- **Extracted URL**: `http://107.175.247.199/loader/install.exe`
- **URLscan.io**: Scan failed — target server unreachable or blocking crawlers. This itself is suspicious (legitimate software does not use bare IPs to serve `.exe` files).
- **URLHaus**: IP `107.175.247.199` is associated with **CoinMiner** and **BitRAT** malware families.
- **VirusTotal**: URL flagged as malicious by multiple security vendors.

## SOC Relevance
Phishing triage is the #1 Tier 1 SOC task. This project demonstrates the ability to analyze email headers, extract IOCs, pivot across threat intelligence platforms, and document findings in an actionable format.

## Tools Used
CyberDefenders, VirusTotal, URLHaus, text editor
