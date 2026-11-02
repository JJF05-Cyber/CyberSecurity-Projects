# Project : Web Application Vulnerability Scan with OWASP ZAP

## Objective
Perform an automated vulnerability scan against DVWA using OWASP ZAP, analyze findings, and map them to the OWASP Top 10.

## Lab Setup
- **Platform**: Kali Linux
- **Target**: DVWA (installed via `apt`, running on `http://127.0.0.1:42001`)
- **Tool**: OWASP ZAP

## Methodology
1. Installed and started DVWA via `sudo apt install dvwa` and `sudo dvwa-start`.
2. Initialized the DVWA database and set security level to Low.
3. Ran ZAP's Automated Scan against `http://127.0.0.1:42001`.
4. Reviewed the 8 alerts returned by the scan and mapped them to OWASP Top 10 categories.
5. Generated an HTML report as evidence.

## Findings

ZAP returned 8 alerts. The highest-risk findings are:

| Vulnerability | Risk | OWASP Top 10 |
|---------------|------|--------------|
| SQL Injection | High | A03: Injection |
| Cross Site Scripting (Reflected) | High | A03: Injection |
| Missing Anti-CSRF Tokens | Medium | A01: Broken Access Control |
| Missing Content Security Policy Header | Medium | A05: Security Misconfiguration |
| Cookie Without HttpOnly Flag | Low | A02: Cryptographic Failures |

### Key Takeaway
ZAP's active scanner successfully identified injection flaws and security misconfigurations in a deliberately vulnerable application. In a SOC context, these findings translate to web attack alerts (SQLi/XSS payloads in HTTP requests) that analysts must recognize in WAF or web server logs.

## SOC Relevance
Web application attacks are a common initial access vector. This project demonstrates:
- Understanding of web vulnerability scanning methodology
- Ability to map findings to OWASP Top 10
- Recognition of attack payloads that appear in web logs

## Tools Used
Kali Linux, OWASP ZAP, DVWA
