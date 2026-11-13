# Project : SSH Brute-Force Detection & Log Analysis

## Objective
Simulate an SSH brute-force attack from Kali Linux against an Ubuntu target (WSL), then analyze the authentication logs to detect and document the attack.

## Lab Setup
- **Attacker**: Kali Linux VM
- **Target**: Ubuntu (WSL) with OpenSSH server
- **Tool**: Hydra (attacker), `/var/log/auth.log` (defender)

## Methodology
1. Started the SSH service on the Ubuntu target and noted its IP address.
2. Created a small password list on Kali Linux.
3. Ran Hydra against the SSH service to simulate a dictionary attack.
4. Monitored `/var/log/auth.log` on the target in real-time.
5. Counted failed attempts per source IP to confirm the brute-force pattern.

## Findings

### Attack Execution
Hydra successfully identified valid credentials by trying passwords from the wordlist. The target logged multiple `Failed password` entries from the Kali IP.

### Log Analysis
Command used:
    sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

**Result:** The Kali IP appeared with multiple failed attempts, confirming the brute-force pattern.

### Detection Opportunity
A SOC analyst would see:
- Multiple `Failed password` events from a single IP within a short time window.
- A threshold alert (e.g., >5 failures in 1 minute) should trigger investigation.
- If a successful login follows a series of failures from the same IP, it indicates a successful compromise.

## SOC Relevance
SSH brute-force is one of the most common alerts in a SOC. This project demonstrates the ability to simulate the attack, recognize its log signature, and document the detection opportunity.

## Tools Used
Kali Linux, Hydra, WSL Ubuntu, OpenSSH, `auth.log`
