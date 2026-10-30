# Project 3: Password Hash Analysis with John the Ripper

## Objective
Demonstrate the ability to identify and crack common password hashes using John the Ripper, and understand the implications for credential-based attacks in a SOC context.

## Lab Setup
- **Platform**: Kali Linux
- **Tool**: John the Ripper (pre-installed)
- **Wordlist**: `/usr/share/wordlists/rockyou.txt`

## Methodology
1. Created a test file containing three MD5 hashes with usernames (`user1:hash`, etc.).
2. Ran John the Ripper with the `raw-md5` format and the `rockyou.txt` wordlist.
3. Displayed the cracked results with `--show`.
4. Repeated the process with a SHA-256 hash to demonstrate format flexibility.

## Findings

### MD5 Cracking

Command:

    john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hashes_named.txt

Result:

    user1:password
    user2:123456
    user3:admin

    3 password hashes cracked, 0 left

All three passwords (`password`, `123456`, `admin`) are extremely common weak passwords that appear early in the `rockyou.txt` wordlist.

### SHA-256 Cracking

Commands:

    echo -n "password" | sha256sum | awk '{print $1}' > sha256.txt
    john --format=raw-sha256 --wordlist=/usr/share/wordlists/rockyou.txt sha256.txt
    john --show --format=raw-sha256 sha256.txt

Result:

    ?:password

    1 password hash cracked, 0 left

Cracked in under one second, confirming that SHA-256 is also too fast for secure password storage.

### Key Takeaway
MD5 and SHA-256 are fast hashing algorithms and are not suitable for password storage. A SOC analyst should recognize credential-dumping alerts (e.g., LSASS access on Windows, `/etc/shadow` access on Linux) and understand how quickly weak passwords can be compromised.

## SOC Relevance
Credential-based attacks are a primary initial access vector. This project demonstrates:
- Understanding of hash formats and cracking methodology
- Recognition of weak password patterns
- Awareness of detection opportunities (e.g., unusual LSASS access, mass authentication failures)

## Tools Used
Kali Linux, John the Ripper, rockyou.txt wordlist
