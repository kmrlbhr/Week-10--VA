# Vulnerability Analysis Labs 1–4
### Target: Metasploitable2 | Attacker: Kali Linux | Tool: Nessus Essentials

---

## Overview

This repository contains the complete laboratory reports for a Vulnerability Analysis assignment covering four progressive labs — from basic scanning to advanced risk prioritization. All labs were conducted in a controlled virtual environment using Kali Linux as the attacker machine and Metasploitable2 as the intentionally vulnerable target.

---

## Lab Environment

| Component | Details |
|-----------|---------|
| Attacker | Kali Linux (VirtualBox VM) |
| Target | Metasploitable2 — `192.168.56.104` |
| Network | Host-Only (VirtualBox) |
| Scanning Tool | Nessus Essentials v10.12.0 |
| Validation Tools | Nmap, Netcat (nc), curl |

---

## Labs Summary

### Lab 1 — Foundation
**Objective:** Perform a basic network vulnerability scan and identify the top 5 vulnerabilities.

- Ran Nessus Basic Network Scan on Metasploitable2
- Discovered **70 vulnerabilities** total
- Identified Top 5 by CVSS score and severity
- Covered key concepts: finding vs real risk, false positives

| # | Vulnerability | CVE | CVSS | Severity |
|---|--------------|-----|------|----------|
| 1 | VNC Server 'password' Password | N/A | 10.0 | Critical |
| 2 | Debian OpenSSH/OpenSSL Weak RNG | CVE-2008-0166 | 10.0 | Critical |
| 3 | Bind Shell Backdoor Detection | N/A | 9.8 | Critical |
| 4 | Apache Tomcat Ghostcat (AJP) | CVE-2020-1938 | 9.8 | Critical |
| 5 | Samba Badlock Vulnerability | CVE-2016-2118 | 7.5 | High |

---

### Lab 2 — Core Analyst Skill
**Objective:** Deep-dive CVE research and CWE mapping for each vulnerability.

- Looked up each vulnerability on CVE.org and NVD
- Identified CVSS Base Score, Attack Vector, Privileges Required
- Mapped each to CWE category
- Assessed exploitability within the lab environment
- Key lesson: **CVSS ≠ actual business risk**

| Vulnerability | CWE | Exploitable? |
|--------------|-----|-------------|
| VNC Weak Password | CWE-521: Weak Password Requirements |  YES |
| OpenSSL Weak RNG | CWE-310: Cryptographic Issues |  YES |
| Bind Shell Backdoor | CWE-912: Hidden Functionality |  YES |
| Ghostcat AJP | CWE-285: Improper Authorization |  YES |
| Samba Badlock | CWE-757: Less-Secure Algorithm Selection |  CONDITIONAL (MITM required) |

---

### Lab 3 — Real-World Scenario
**Objective:** Manually validate three scanner findings and classify as True Positive / False Positive / Accepted Risk.

- Used Nmap, Netcat, and curl for manual validation
- Key lesson: **Blind reporting = bad analyst**

| Scenario | Finding | Verdict | Evidence |
|----------|---------|---------|----------|
| SSL Weak Cipher | SSL v2/v3 reported by scanner |  Accepted Risk | Port 443/8443 CLOSED — no HTTPS on standard ports |
| Open Port No Auth | Bind shell + Telnet without auth |  True Positive | `nc port 1524 → uid=0(root)` immediately |
| Outdated Service | vsftpd 2.3.4, Apache 2.2.8, PHP 5.2.4 |  True Positive | Banner grab confirmed all versions; all EOL |

---

### Lab 4 — Advanced
**Objective:** Risk-based prioritization and remediation planning.

- Ranked vulnerabilities using custom scoring: **Exploitability + Impact + Exposure (max 30)**
- Created remediation priority list with timeframes
- Explained why Medium CVSS can be more dangerous than High CVSS

| Rank | Vulnerability | CVSS | Risk Score | Priority | Timeframe |
|------|--------------|------|-----------|----------|-----------|
| 1 | Bind Shell Backdoor | 9.8 | 30/30 | IMMEDIATE | 24 hours |
| 2 | VNC Weak Password | 10.0 | 28/30 | IMMEDIATE | 24 hours |
| 3 | OpenSSL Weak RNG | 10.0 | 25/30 | CRITICAL | 48 hours |
| 4 | Ghostcat AJP | 9.8 | 23/30 | HIGH | 1 week |
| 5 | Samba Badlock | 7.5 | 18/30 | MEDIUM | 2 weeks |

> **Note:** VNC has a higher CVSS (10.0) than Bind Shell (9.8) but ranks lower — Bind Shell provides direct unauthenticated root command execution, confirmed with `uid=0(root)` evidence.
