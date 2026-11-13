# Incident Report: Multiple Failed Logins Followed by Success

**Incident ID:** INC-2026-001  
**Event Source:** Windows Security Log  
**Relevant Event IDs:** 4625 (Failed Logon), 4624 (Successful Logon)  
**Severity:** Low (Simulated Lab)  
**Status:** Closed

## 1. Evidence Collected
- Multiple **Event ID 4625** entries indicating failed authentication attempts.
- The events originated from the local machine (or internal IP).
- A subsequent **Event ID 4624** indicating a successful authentication for the same user account shortly after the failures.

## 2. Analysis
The log sequence shows multiple failed password attempts followed immediately by a successful login. In a production environment, this pattern is a primary indicator of a **brute-force attack** or a **password-guessing attempt**. 

However, because this activity originated from the local console and was followed by a legitimate login by the user (me), it is most likely a case of a user mistyping their password multiple times (User Error) rather than a malicious remote attack. 

## 3. Recommended Response (SOC Playbook)
1. **Verify Source IP:** Check if the failed logins originated from an external IP. If yes, immediately block the IP at the firewall.
2. **Contact User:** Verify with the account owner if they were the ones attempting to log in.
3. **Check for Lateral Movement:** If the login was malicious, review subsequent logs to see if the attacker attempted to escalate privileges or move laterally.
4. **Account Lockout:** If the threshold of failures exceeded the company policy (e.g., 10 attempts), ensure the account is locked and force a password reset.
