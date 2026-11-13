# Project : SSH Brute-Force Detection & Log Analysis

## Objective
Simulate an SSH brute-force attack from Kali Linux against an Ubuntu target (WSL), then analyze the authentication logs to detect and document the attack.

## Lab Setup
- **Attacker**: Kali Linux VM (Connected via a **Wi-Fi Network Adapter** in Bridged mode)
- **Target**: Ubuntu (WSL) with OpenSSH server (Port forwarded via the Windows Host)
- **Tools**: Hydra (Attacker), `/var/log/auth.log` (Defender)

## Methodology
1. Configured the Kali Linux VM to use a physical **Wi-Fi Network Adapter** in Bridged mode to place it on the same local network as the Windows host.
2. Configured a Windows Port Proxy (`netsh interface portproxy`) on the host to forward traffic from port `2222` to the WSL SSH service on port `22` (overcoming WSL2 network isolation).
3. Created a small password list (`pass.txt`) on Kali Linux.
4. Ran Hydra against the Windows host IP on port `2222` to simulate a dictionary attack.
5. Monitored `/var/log/auth.log` on the WSL target to identify the failed login attempts.
6. Counted failed attempts per source IP to confirm the brute-force pattern.

## Findings

### Attack Execution
Hydra successfully connected to the forwarded port and attempted 4 password guesses against the SSH service. While the attack did not find valid credentials, it successfully generated multiple authentication failures in the target's log.

### Log Analysis
Command used:
    sudo grep "Failed password" /var/log/auth.log | awk '{print $11}' | sort | uniq -c | sort -nr

**Result**: The log analysis identified the Kali Linux Wi-Fi adapter's IP (`192.168.0.189`) with 4 failed attempts, confirming the brute-force pattern.

### Detection Opportunity
A SOC analyst would see:
- Multiple `Failed password` events from a single IP within a short time window.
- A threshold alert (e.g., >5 failures in 1 minute) should trigger investigation.
- If a successful login follows a series of failures from the same IP, it indicates a successful compromise.

## SOC Relevance
SSH brute-force is one of the most common alerts in a SOC. This project demonstrates the ability to simulate the attack, recognize its log signature, and document the detection opportunity. Furthermore, it highlights the ability to troubleshoot cross-platform networking (Bridged Wi-Fi and WSL Port Forwarding) to establish a testing environment.

## Tools Used
Kali Linux, Hydra, WSL Ubuntu, OpenSSH, Linux CLI (`grep`, `awk`, `uniq`), Windows `netsh`.
