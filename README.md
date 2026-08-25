# Web Application Security Assessment — DVWA

A controlled web application security assessment performed against **Damn Vulnerable Web Application (DVWA)** running locally on Kali Linux. The project simulates a simplified penetration-testing workflow using Burp Suite to identify, validate, and document common web vulnerabilities in a deliberately vulnerable environment.

## Summary

Three vulnerabilities were identified and confirmed:

| ID | Finding | Severity | Status |
|----|---------|----------|--------|
| F-01 | SQL Injection | High | Confirmed |
| F-02 | Reflected Cross-Site Scripting (XSS) | Medium | Confirmed |
| F-03 | OS Command Injection | High | Confirmed |

All testing was performed against a local, intentionally vulnerable instance. No production systems, real user data, or Internet-facing targets were involved.

## Lab Environment

| Item | Details |
|---|---|
| Platform | Kali Linux |
| Web Server | Nginx |
| Target Application | DVWA |
| Testing Tool | Burp Suite |
| DVWA Security Level | Low |
| Target Address | `127.0.0.1:42001` |

Start DVWA on Kali with:

```bash
sudo dvwa-start
```

Application accessed at `http://127.0.0.1:42001`.

## Objectives

- Set up a deliberately vulnerable web application
- Configure Burp Suite as an interception proxy
- Capture and analyze HTTP requests/responses
- Test application inputs for common vulnerabilities
- Validate vulnerabilities using controlled payloads
- Collect HTTP evidence
- Assess security impact and recommend remediation
- Document the complete assessment

## Methodology

```
Lab Setup → Application Discovery → Burp Suite Configuration →
Request Interception → Input Testing → Vulnerability Validation →
Evidence Collection → Risk Assessment → Remediation
```

Burp Suite was configured as the HTTP proxy (`127.0.0.1:8080`) between the browser and DVWA, with traffic visible under **Proxy → HTTP history** and requests sent to **Repeater** for controlled modification.

## Findings

### F-01 — SQL Injection (`/vulnerabilities/sqli/`)
**Severity: High**
Payload `1' OR '1'='1` returned multiple database records, confirming user input was incorporated into the SQL query without parameterization.

**Remediation:** parameterized queries/prepared statements, server-side input validation, least-privilege database accounts, avoid exposing DB errors.

### F-02 — Reflected XSS (`/vulnerabilities/xss_r/`)
**Severity: Medium**
Payload `<script>alert('XSS')</script>` was reflected unescaped in the HTML response and executed in the browser.

**Remediation:** context-aware output encoding, HTML escaping of untrusted input, safe templating, Content Security Policy.

### F-03 — OS Command Injection (`/vulnerabilities/exec/`)
**Severity: High**
Input `127.0.0.1; whoami` executed alongside the intended ping command, confirming the application passed unsanitized input to a shell.

**Remediation:** avoid passing user input to shell commands, use shell-free APIs, allowlist validation, run with least privilege.

## Evidence

Screenshots documenting each confirmed finding:
- `01-sqli-confirmed.png`
- `02-xss-confirmed.png`
- `03-command-injection.png`

## Limitations

- Target was an intentionally vulnerable application tested locally, at Low security level
- Only three vulnerabilities were assessed; no automated exploitation or post-exploitation activity
- No production application, real user data, or Internet-facing target was involved
- Findings demonstrate security testing technique, not evidence of real-world compromise

## Ethical Considerations

All testing was performed against a deliberately vulnerable, locally hosted application for educational and skill-development purposes. No unauthorized systems, accounts, or networks were targeted. DVWA should remain restricted to a controlled lab environment.

## Skills Demonstrated

Web application security testing · Burp Suite · HTTP request/response analysis · parameter manipulation · SQL injection, XSS, and command injection testing · vulnerability validation · evidence collection · risk assessment · remediation planning · technical reporting

## Conclusion

Using Kali Linux, DVWA, and Burp Suite, this project walks through a structured assessment — from request interception through vulnerability validation, evidence collection, risk assessment, and remediation — demonstrating the ability to analyze application behavior and communicate security findings in a professional format.
