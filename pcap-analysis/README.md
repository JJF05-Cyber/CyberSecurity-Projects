# Project 4: Network Traffic Analysis (PCAP) — Koi Stealer C2 Investigation

## Objective
Analyze a malicious PCAP to identify the infected host, the command-and-control (C2) communication, and the likely initial infection vector.

## Lab Setup
- **Platform**: Kali Linux
- **Tool**: Wireshark
- **Sample Source**: malware-traffic-analysis.net (public training exercise)

## Methodology
1. Opened the PCAP in Wireshark.
2. Filtered for `http.request` to identify suspicious outbound traffic.
3. Filtered for `dns` to spot malicious domain lookups.
4. Used "Follow TCP Stream" to reconstruct the C2 conversation.
5. Correlated the C2 IP with OSINT (VirusTotal, AbuseIPDB, public research).

## Findings

### Victim and C2
- **Victim IP**: 172.17.0.99
- **C2 IP**: 79.124.78.197
- **C2 Path**: `/foots.php`
- **Malware Family**: Koi Stealer

### C2 Beacon Analysis
The victim sent a POST request to a bare IP address (no domain), which is a classic C2 pattern used to evade DNS-based blocking.

Request characteristics:
- `Host: 79.124.78.197` (bare IP, no domain)
- `Content-Type: application/octet-stream` (raw binary, not a normal browser form)
- `User-Agent: Mozilla/4.0 (compatible; MSIE 7.0; ... Windows NT 10.0 ...)` (spoofed legacy IE7 UA on a Windows 10 host)
- Payload: `102|30168213-3be0-a751-81a0-cf3b228c8654` (command ID + agent UUID)
- Response: `HTTP/1.1 200 OK` with an empty body (server had no pending commands)

This matches the known Koi Stealer check-in format: the malware reports to the C2 with a structured identifier, and the server acknowledges without issuing new tasks.

### Pre-Infection Analysis
Before C2 beaconing began, the victim established a TLS connection to `www.bellantonicioccolato.it`. This domain has been identified by OSINT as a compromised website used for malware distribution, associated with the WikiLoader (DBatLoader) loader family. Although the encrypted traffic prevents direct observation of the downloaded payload, the timing and threat intelligence correlation strongly suggest this is the initial infection vector.

### Threat Intelligence
- **VirusTotal**: 79.124.78.197 flagged as malicious by 1/93 engines.
- **AbuseIPDB**: No current risk score — common for rotating C2 infrastructure.
- **Attribution**: Public research confirms this IP and `/foots.php` path as Koi Stealer C2 infrastructure.

## Attack Chain
1. Victim visited compromised website `www.bellantonicioccolato.it`.
2. WikiLoader (first-stage loader) was downloaded and executed.
3. WikiLoader retrieved the second-stage payload: Koi Stealer.
4. Koi Stealer beaconed to `79.124.78.197/foots.php` with its agent UUID.

## SOC Relevance
PCAP analysis is a core SOC skill for investigating network alerts. This project demonstrates the ability to:
- Filter and analyze network traffic in Wireshark
- Identify C2 communication patterns (bare IP, octet-stream, structured beacon)
- Recognize spoofed User-Agents
- Pivot to OSINT to attribute malware families

## Tools Used
Kali Linux, Wireshark, VirusTotal, AbuseIPDB
