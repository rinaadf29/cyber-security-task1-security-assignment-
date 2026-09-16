# Cybersecurity Foundational Assessment Report

**Client (fictional):** Northwind Craft Supplies Pvt. Ltd.
**Engagement:** Task 1 — Foundational Security Assessment of a Small Organization
**Prepared by:** Junior Cybersecurity Analyst
**Date:** September 2026
**Classification:** Internal — Confidential
**Version:** 1.0

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Introduction and Scope](#2-introduction-and-scope)
3. [Methodology](#3-methodology)
4. [Asset Inventory and Classification](#4-asset-inventory-and-classification)
5. [Threat and Vulnerability Assessment](#5-threat-and-vulnerability-assessment)
6. [CIA Triad Impact Assessment](#6-cia-triad-impact-assessment)
7. [Network Architecture](#7-network-architecture)
8. [Protocols and Secure Communication](#8-protocols-and-secure-communication)
9. [Recommended Security Controls](#9-recommended-security-controls)
10. [Endpoint Hardening](#10-endpoint-hardening)
11. [Remediation Roadmap](#11-remediation-roadmap)
12. [Conclusion](#12-conclusion)
13. [References](#13-references)
14. [Appendix — Glossary](#14-appendix--glossary)

---

## 1. Executive Summary

Northwind Craft Supplies is a 25-person business that depends entirely on digital systems: a file server holding all operational documents, a public web portal that processes customer orders, and a customer database containing approximately 8,000 records of personal data. This assessment reviewed that environment and found a security posture typical of small organizations that have grown faster than their IT practices — functional, but without the basic protective layers that modern threats assume are absent.

**Twelve assets** were inventoried, **fourteen threats** and **sixteen vulnerabilities** identified, and **thirteen risks** registered. Of those risks, **three are rated Critical** and **six are rated High**.

The three Critical risks are:

| Risk | Description | Score |
|---|---|---|
| **R-01** | Ransomware encrypts the file server *and* the writable, co-located backup NAS, leaving no recovery path | 20/25 |
| **R-02** | SQL injection in the customer order portal exposes 8,000 personal records, triggering DPDP Act 2023 notification duties | 20/25 |
| **R-03** | Phishing captures Microsoft 365 credentials in the absence of multi-factor authentication, enabling invoice fraud | 20/25 |

These three risks share a single root cause pattern: **identity is unprotected, systems are unpatched, and recovery is unverified.**

**The most important finding of this assessment is a positive one.** The majority of the identified risk can be eliminated at effectively zero financial cost. Enabling multi-factor authentication, closing the firewall's exposed administrative interface and forwarded RDP port, removing local administrator rights, applying outstanding patches, isolating the backup target, and fixing input validation in the portal are all configuration and discipline changes — not purchases. Northwind's gap is not a budget problem; it is an operational-practice problem.

**Headline recommendation:** execute Phase 1 (one week, near-zero cost) and Phase 2 (three weeks, low cost) of the remediation roadmap in Section 11. Together they reduce the profile from *3 Critical / 6 High* to approximately *0 Critical / 3 High*.

---

## 2. Introduction and Scope

### 2.1 Purpose

This report documents a foundational security assessment carried out as an academic exercise. Its objectives are to establish practical competence in asset management, threat and vulnerability analysis, the CIA triad, network architecture, the OSI and TCP/IP models, core protocols, secure communication, and endpoint hardening.

### 2.2 The environment assessed

| Component | Detail |
|---|---|
| Employees | 25 |
| Sites | 1 office |
| Endpoints | 15 laptops (12 Windows, 3 macOS) |
| Servers | 1 on-premises file server |
| Network | 1 router/firewall, 1 managed switch, 2 Wi-Fi access points |
| Public services | 1 website with customer order portal (cloud-hosted) |
| Connectivity | Single ISP fibre circuit, 200 Mbps |
| Cloud services | Microsoft 365 (email, documents) |

### 2.3 In scope

Asset identification and classification; threat modelling; vulnerability identification through configuration review; risk analysis; CIA impact assessment; network architecture review and redesign; protocol and secure-communication analysis; control recommendations; endpoint-hardening guidance.

### 2.4 Out of scope

Active vulnerability scanning, penetration testing, exploitation, social-engineering exercises, physical intrusion testing, source-code review, and formal regulatory compliance audit.

### 2.5 Ethical statement

All systems described in this report are **fictional**. No live system was scanned, probed, exploited or tested. No credentials were collected or attempted. No personal data was gathered. Where techniques are described (SQL injection, DHCP spoofing, DNS hijacking), they are explained at a conceptual level for defensive understanding only, and contain no operational detail that would assist an attacker. Any future practical work associated with this task will be conducted exclusively within intentionally vulnerable training environments, published CTF platforms, or self-built laboratory systems on hardware owned by the author.

### 2.6 Limitations

The assessment relies on configuration review and interview rather than technical testing, so vulnerabilities requiring active verification may exist beyond those listed. Likelihood ratings are qualitative and based on published industry incident patterns rather than on Northwind's own historical data, which does not exist because no logging has been retained (V-11).

---

## 3. Methodology

The assessment followed a structured five-stage process aligned with the **NIST Cybersecurity Framework 2.0** functions (Govern, Identify, Protect, Detect, Respond, Recover) and the **CIS Critical Security Controls v8** Implementation Group 1, which is specifically designed for small organizations with limited security expertise.

| Stage | Activity | Framework mapping | Output |
|---|---|---|---|
| **1. Identify** | Inventory all hardware, software, data, network, service and identity assets; classify by type and business criticality | NIST *Identify*; CIS Controls 1–3 | Document 01 |
| **2. Analyse** | Model threats using STRIDE; identify vulnerabilities through configuration review; calculate risk as Likelihood × Impact | NIST *Identify* (Risk Assessment) | Document 02 |
| **3. Assess impact** | Rate each major asset against Confidentiality, Integrity and Availability | ISO/IEC 27001 Annex A | Document 03 |
| **4. Design** | Map the current network, identify architectural weaknesses, produce a segmented target design and protocol analysis | NIST *Protect*; CIS Controls 12–13 | Documents 04–05 |
| **5. Recommend** | Select layered controls, prioritise by risk reduction per unit of cost, produce a phased roadmap and operational checklist | NIST *Protect / Detect / Recover* | Documents 06–07 |

**Risk scoring model.** Likelihood and Impact are each rated 1–5. Risk = Likelihood × Impact, producing a score of 1–25 mapped to four treatment bands (Critical ≥ 20, High 12–19, Medium 6–11, Low ≤ 5). Ratings were assigned based on published small-business incident frequency data and on the business consequence described by the asset owner.

---

## 4. Asset Inventory and Classification

**Full detail:** [Document 01](01-asset-inventory.md)

Twelve assets were identified across six categories. The distribution by criticality:

| Importance | Count | Assets |
|---|---|---|
| **Critical** | 5 | File server, router/firewall, public web service, customer database, accounts/credentials |
| **High** | 6 | Laptops, Wi-Fi APs, Internet connectivity, domain/DNS, backup NAS, business email |
| **Medium** | 1 | Network switch |

Four findings emerged directly from the inventory exercise:

1. **Single points of failure exist at the edge.** One firewall and one ISP circuit. Failure of either takes the office offline with no fallback.
2. **The backup is not independent.** The NAS sits in the same room as the server it protects, on the same LAN, mounted as a writable share. It fails in the same fire, the same theft and the same ransomware event as the primary — meaning Northwind effectively has one copy of its data, not two.
3. **Identity is the highest-leverage asset.** Compromise of one shared administrator credential grants access to the file server, firewall, switch, access points and email simultaneously.
4. **An inventory gap was found during the walkthrough.** Personal mobile phones are used to read company email but are not centrally managed, enrolled or wipeable. A BYOD decision is required.

---

## 5. Threat and Vulnerability Assessment

**Full detail:** [Document 02](02-threat-vulnerability-assessment.md)

### 5.1 Conceptual foundation

Precise use of these four terms is what distinguishes analysis from anxiety:

- A **threat** is a potential cause of harm. It exists independently of you and cannot be removed — ransomware operators will not stop existing because Northwind patches a server.
- A **vulnerability** is a weakness that a threat can exploit. This is the only one of the four that the organization directly fixes.
- **Risk** is the measurement produced by combining the likelihood of successful exploitation with the business impact. It is a number used to prioritise, not an object.
- A **security control** is a safeguard that reduces risk — by lowering likelihood, lowering impact, or improving detection and recovery speed.

Expressed as a relationship: **a threat exploits a vulnerability to create a risk, which a control reduces.** Understanding this chain is what prevents the two most common small-business errors — buying a product to solve a process problem, and treating the absence of past incidents as evidence of security.

### 5.2 Threats identified (14)

Phishing and business email compromise; ransomware; laptop theft or loss; credential stuffing and password spraying; malware via removable media or drive-by download; malicious or negligent insider; Wi-Fi eavesdropping and rogue access points; router/firewall compromise; web application attack (SQL injection, XSS); distributed denial of service; hardware failure, power loss or fire; man-in-the-middle and DNS hijacking; supply-chain compromise; physical intrusion and tailgating.

### 5.3 Vulnerabilities identified (16)

Four vulnerabilities were rated **Critical**:

| ID | Vulnerability | Why it is critical |
|---|---|---|
| **V-01** | Missing OS and application patches; two end-of-life Windows builds | Publicly documented exploits exist for known CVEs; no vendor fix is coming for EOL systems |
| **V-02** | No multi-factor authentication on any system | A single stolen password grants full access to email, VPN and cloud administration |
| **V-03** | Weak, reused and shared administrator passwords | Eliminates both resistance to guessing and the ability to attribute any action to a person |
| **V-07** | Default credentials on the switch; firewall admin page exposed to the Internet | An attacker needs no exploit at all — only the vendor's published default password |
| **V-09** | Untested, writable, co-located backups | Removes the last line of defence precisely when it is needed |
| **V-13** | Unvalidated SQL input in the order portal | Directly exposes 8,000 personal records; SQL injection remains among the most exploited web weaknesses |

Twelve further vulnerabilities were rated High or Medium, covering privilege management, network segmentation, disk encryption, file-share permissions, endpoint detection, logging, transport security, wireless configuration, email authentication and security awareness.

### 5.4 Risk register summary

| Rating | Count | Remediation deadline |
|---|---|---|
| **Critical** | 3 | 7 days |
| **High** | 6 | 30 days |
| **Medium** | 4 | 90 days |
| **Low** | 0 | — |

The absence of any Low-rated risks is itself a finding: no identified issue is safe to accept in its current state.

---

## 6. CIA Triad Impact Assessment

**Full detail:** [Document 03](03-cia-triad-assessment.md)

**Confidentiality** ensures information is disclosed only to authorized parties. **Integrity** ensures information is accurate and unaltered. **Availability** ensures information is accessible when needed. Every control in this report protects at least one of these three, and the three routinely conflict — maximising one at the expense of the others is the most common design mistake in small environments.

### Summary ratings

| Asset | C | I | A | Dominant concern |
|---|---|---|---|---|
| Employee laptops | H | M | M | Unencrypted local data on a stolen device |
| File server | H | H | H | All operational data, over-permissive access, 4-hour RTO |
| Router / firewall | H | H | H | Single point of total network failure |
| Network switch | M | H | H | Management access enables undetected traffic capture |
| Wi-Fi access points | H | M | M | Shared, never-rotated passphrase on a flat network |
| Public web service | H | H | H | Primary sales channel, Internet-exposed |
| **Customer database** | **H** | **H** | **H** | **Crown jewel — 8,000 records with regulatory exposure** |
| Domain / DNS | M | H | H | Hijacking redirects customers while servers stay healthy |
| Backup NAS | H | H | H | Last line of defence, currently not independent |
| Business email | H | H | M | Richest single source for invoice fraud |
| Accounts / credentials | H | H | M | Force multiplier across every other asset |

### Three conclusions

1. **Availability is the weakest of the three properties.** No backup independence, no ISP failover, no spare firewall. A fire in the server cupboard is currently an existential event for this business.
2. **Integrity is the least monitored.** With no audit-log review and no file-integrity monitoring, an integrity failure would most likely be discovered by a customer complaint rather than by Northwind.
3. **Confidentiality carries the regulatory weight.** The customer database falls under India's Digital Personal Data Protection Act 2023, which imposes notification obligations and penalty exposure that the other assets do not carry.

---

## 7. Network Architecture

**Full detail and diagrams:** [Document 04](04-network-architecture.md) · [SVG diagram](../diagrams/network-diagram.svg)

### 7.1 Current state

Northwind operates a **flat network**: every device — employee laptops, the file server, the backup NAS, the switch management interface and guest Wi-Fi clients — shares the single subnet `192.168.1.0/24` and one broadcast domain. The public web service is cloud-hosted and reached over the Internet.

Seven architectural weaknesses were identified: guest devices can reach the file server directly; switch management shares a subnet with users; the backup NAS is a writable target on the production LAN; the firewall administration page is reachable from the Internet; there is no DMZ or WAF in front of the public portal; the edge has no redundancy; and there is no network monitoring whatsoever.

The practical consequence is that **a single compromised device — a visitor's infected phone on guest Wi-Fi, or one laptop that opened the wrong attachment — has unobstructed Layer 2 access to every critical asset in the building.** In a flat network there is no such thing as a contained incident.

### 7.2 Target state

The recommended design introduces five VLANs with default-deny routing between them:

| VLAN | Purpose | Subnet |
|---|---|---|
| 10 | Corporate endpoints | 192.168.10.0/24 |
| 20 | Servers | 192.168.20.0/24 |
| 30 | Guest Wi-Fi (Internet only) | 192.168.30.0/24 |
| 40 | Backup (pull-based, isolated) | 192.168.40.0/24 |
| 99 | Device management (out-of-band) | 192.168.99.0/24 |

Key design decisions:

- **Guest traffic is denied to every internal VLAN** without exception. Guests receive Internet access and nothing else, with client isolation enabled so guest devices cannot see each other.
- **The backup VLAN pulls from the server rather than the server pushing to it.** This single inversion means that ransomware executing with the server's privileges cannot write to the backup target — the defining control against R-01.
- **Device management lives on VLAN 99**, reachable only from a jump host requiring MFA. The management plane is separated from the data plane.
- **Subnets are aligned to VLAN IDs** (VLAN 10 → `192.168.10.0/24`), so an IP address alone identifies its security zone during an incident.

### 7.3 OSI and TCP/IP mapping

Attacks occur at every layer, so controls must exist at every layer. TLS at Layer 6 does nothing against an unlocked server cupboard at Layer 1; a firewall at Layers 3–4 does nothing against SQL injection at Layer 7. The full seven-layer control mapping is given in Document 04 §4.4 — it is the technical justification for the defence-in-depth model used in Section 9.

---

## 8. Protocols and Secure Communication

**Full detail:** [Document 05](05-protocols-and-secure-communication.md)

| Protocol | Role at Northwind | Principal risk | Control |
|---|---|---|---|
| **IP addressing** | Identifies and routes to every device; subnet boundaries enforce segmentation | Spoofing, flat addressing defeating segmentation | Anti-spoofing ACLs, VLAN-aligned subnet plan |
| **DNS** | Resolves `northwindcraft.example` for customers and email routing | Registrar hijacking — customers reach an attacker while Northwind's servers log nothing unusual | Registrar MFA and lock, DNSSEC, filtering resolver, change monitoring |
| **DHCP** | Assigns addresses via the DORA exchange; 8-hour corporate and 4-hour guest leases | Rogue DHCP server becomes the gateway and DNS for the whole subnet — instant man-in-the-middle | DHCP snooping with trusted ports, port security, dynamic ARP inspection |
| **TCP** | Reliable transport for HTTPS, SMB, SSH via the three-way handshake | Exposed management ports (3389 currently forwarded), SYN flooding | Default-deny inbound, VPN with MFA for remote access, SYN cookies |
| **UDP** | Fast transport for DNS, DHCP, NTP and VPN | Source-address spoofing enables amplification DDoS | BCP 38 source validation, rate limiting, no open resolvers |
| **HTTP** | Legacy plaintext web access; one portal path still uses it | Every field — passwords, session cookies, addresses — readable and modifiable in transit | Redirect all HTTP to HTTPS with a 301 |
| **HTTPS / TLS** | Protects all customer and staff web traffic | Certificate expired 3 weeks ago; no HSTS; possible legacy TLS versions enabled | TLS 1.3, automated ACME renewal, HSTS preload, secure cookie flags, security headers |

**How TLS delivers three properties at once:** the server's X.509 certificate proves domain control (authentication); ephemeral Diffie-Hellman key exchange derives a symmetric session key that is never transmitted (confidentiality, with forward secrecy); and AEAD authentication tags detect any modification in transit (integrity). Asymmetric cryptography is used only to establish trust and agree the key — the data itself is carried by fast symmetric encryption.

**The most urgent protocol finding is also the simplest to fix.** Northwind's TLS certificate expired three weeks ago. Every customer currently sees a full-page browser warning before reaching the order portal. This costs revenue immediately, and worse, it trains customers to click through security warnings — which is precisely the habit an attacker running a phishing site relies on.

---

## 9. Recommended Security Controls

**Full detail:** [Document 06](06-security-controls.md)

Fifteen controls are recommended across four domains.

| Domain | Controls |
|---|---|
| **Endpoint** | C-07 patch management · C-08 encryption and device control · C-10 EDR and logging |
| **Network** | C-03 firewall hardening · C-04 segmentation and wireless · C-10 monitoring · C-11 web and transport security |
| **Account** | C-01 password policy · C-02 multi-factor authentication · C-05 least privilege · C-12 email security |
| **Data** | C-06 access control and classification · C-08 encryption · C-09 backup and recovery · C-11 transport security |
| **People and governance** | C-13 awareness training · C-14 physical security · C-15 incident response plan |

### Defence in depth

Controls are layered so that an attacker must defeat several independent barriers in sequence. A phishing email that gets past the mail filter meets MFA at the identity layer; a payload that survives MFA meets EDR at the endpoint layer; malware that executes meets network segmentation before it reaches servers; and if all of that fails, verified immutable backups restore operations. No single control is expected to hold — the design assumes each will eventually fail.

### The highest-return control

**C-02 (multi-factor authentication)** is the single most valuable recommendation in this report. It directly reduces three of the thirteen registered risks, costs nothing on Microsoft 365, and can be enabled in under an hour. Because credentials underwrite the confidentiality and integrity of every other asset, MFA is the control with the widest blast radius of protection.

---

## 10. Endpoint Hardening

**Full detail:** [Document 07](07-endpoint-hardening-checklist.md)

An eighty-item checklist covers nine areas: updates and patching; passwords and authentication; multi-factor authentication; host firewall; antivirus and EDR; least privilege; backup and recovery; data protection and encryption; and configuration and monitoring.

If time is limited, seven actions remove the majority of realistic endpoint risk, in this order:

1. Enable MFA on all cloud accounts
2. Remove local administrator rights from standard users
3. Apply outstanding patches and enable auto-update
4. Turn on full-disk encryption (BitLocker / FileVault)
5. Install and verify EDR with tamper protection enabled
6. Enable the host firewall with default-deny inbound
7. **Verify that a backup restore actually works**

Item 7 deserves emphasis. Northwind has backups. What it does not have is evidence that those backups can be restored — the NAS copy has never been test-restored in its operational life. An unverified backup is not a control; it is an assumption. Many organizations discover this distinction during the worst week of their existence.

Every one of these seven actions is free or near-free, and none requires new hardware.

---

## 11. Remediation Roadmap

| Phase | Timeframe | Key actions | Risks addressed | Cost |
|---|---|---|---|---|
| **1. Emergency** | Week 1 | Enable MFA; close the WAN-facing firewall admin page and forwarded port 3389; change all default credentials; renew the TLS certificate and force HTTPS; fix the SQL injection with parameterised queries | R-02, R-03, R-04, R-10 | ~Zero |
| **2. Foundation** | Weeks 2–4 | Remove local admin rights; retire shared accounts; patch all systems; isolate the backup and run the first verified test restore; enable full-disk encryption | R-01, R-05, R-06, R-11 | Low |
| **3. Architecture** | Months 2–3 | Implement VLAN segmentation; upgrade Wi-Fi to WPA3; rebuild file-share permissions; deploy EDR and central logging; publish DKIM and DMARC | R-07, R-08, R-09, R-13 | Medium |
| **4. Maturity** | Months 4–6 | Security awareness programme; physical security improvements; incident response plan and tabletop exercise; WAF/CDN; ISP failover; annual authorised penetration test | Residual risk, R-12 | Medium |

### Projected risk reduction

| Milestone | Critical | High | Medium |
|---|---|---|---|
| Today | 3 | 6 | 4 |
| After Phase 1 | 1 | 5 | 4 |
| After Phase 2 | 0 | 3 | 5 |
| After Phase 4 | 0 | 1 | 3 |

### Suggested measures of success

| Metric | Today | 6-month target |
|---|---|---|
| MFA coverage on privileged accounts | 0% | 100% |
| Endpoints with EDR installed | 0% | 100% |
| Endpoints with full-disk encryption | 0% | 100% |
| Critical patches applied within 7 days | Unmeasured | > 95% |
| Verified test restores completed | 0 | 2 (quarterly) |
| Staff completing security training | 0% | 100% |
| Phishing simulation click rate | Unmeasured | < 10% |

---

## 12. Conclusion

Northwind Craft Supplies operates a functional but structurally fragile IT environment. Twelve assets, fourteen threats and sixteen vulnerabilities produced thirteen registered risks, three of them Critical. Any one of those three — ransomware with no recoverable backup, a database breach with regulatory consequences, or credential theft enabling invoice fraud — is capable of ending the business rather than merely inconveniencing it.

The environment's weaknesses are not unusual or sophisticated. They are the familiar pattern of an organization whose IT grew alongside the business without a deliberate security design: one flat network because it was simplest to cable; shared administrator passwords because it was convenient; a backup on the nearest spare device because it was available; no MFA because nobody had asked for it. Each decision was reasonable in isolation. Collectively they produce an environment in which a single compromised device reaches every critical asset.

Three principles should guide the response. **Least privilege** — nobody and nothing gets more access than the task requires; this alone contains most incidents at the point of entry. **Defence in depth** — assume every individual control will eventually fail, and layer accordingly. **Verify, do not assume** — a backup that has never been restored, a patch that was scheduled but not confirmed, and a firewall rule that nobody has reviewed in three years are all assumptions wearing the costume of a control.

The encouraging conclusion is one of cost. Phases 1 and 2 of the roadmap — four weeks of work, at essentially no financial outlay — eliminate all three Critical risks and half the High risks. For a small business, the distance between the current posture and a reasonably defensible one is measured in operational discipline far more than in budget.

---

## 13. References

1. NIST, *Cybersecurity Framework (CSF) 2.0*, NIST CSWP 29, 2024.
2. NIST, *SP 800-53 Rev. 5: Security and Privacy Controls for Information Systems and Organizations*, 2020.
3. NIST, *SP 800-63B: Digital Identity Guidelines — Authentication and Lifecycle Management*.
4. NIST, *SP 800-61: Computer Security Incident Handling Guide*.
5. NIST, *SP 800-30 Rev. 1: Guide for Conducting Risk Assessments*, 2012.
6. Center for Internet Security, *CIS Critical Security Controls v8*, Implementation Group 1.
7. ISO/IEC 27001:2022, *Information security, cybersecurity and privacy protection — Information security management systems*.
8. OWASP Foundation, *OWASP Top 10 Web Application Security Risks*.
9. OWASP Foundation, *Cheat Sheet Series* — SQL Injection Prevention, Transport Layer Security, Session Management.
10. MITRE, *ATT&CK Enterprise Matrix* — adversary tactics and techniques.
11. IETF RFC 8446, *The Transport Layer Security (TLS) Protocol Version 1.3*, 2018.
12. IETF RFC 2131, *Dynamic Host Configuration Protocol*.
13. IETF RFC 1034 / RFC 1035, *Domain Names — Concepts, Facilities and Specification*.
14. IETF RFC 793 / RFC 9293, *Transmission Control Protocol*.
15. IETF RFC 768, *User Datagram Protocol*.
16. IETF BCP 38 / RFC 2827, *Network Ingress Filtering*.
17. IETF RFC 1918, *Address Allocation for Private Internets*; RFC 5737, *IPv4 Address Blocks Reserved for Documentation*.
18. ISO/IEC 7498-1, *Open Systems Interconnection — Basic Reference Model*.
19. Government of India, *Digital Personal Data Protection Act, 2023*.
20. Verizon, *Data Breach Investigations Report* — small-business incident patterns.

---

## 14. Appendix — Glossary

| Term | Definition |
|---|---|
| **ACL** | Access Control List — rules permitting or denying traffic or access |
| **AEAD** | Authenticated Encryption with Associated Data — provides confidentiality and integrity together |
| **BEC** | Business Email Compromise — fraud carried out through a compromised or spoofed mailbox |
| **CIA Triad** | Confidentiality, Integrity, Availability — the three core security properties |
| **CVE** | Common Vulnerabilities and Exposures — the public identifier system for known flaws |
| **DHCP** | Dynamic Host Configuration Protocol — automatic IP address assignment |
| **DMARC** | Domain-based Message Authentication, Reporting and Conformance — email anti-spoofing policy |
| **DMZ** | Demilitarised Zone — a network segment for Internet-facing services |
| **DNS** | Domain Name System — resolves names to IP addresses |
| **DNSSEC** | DNS Security Extensions — cryptographic signing of DNS responses |
| **DoS / DDoS** | Denial of Service / Distributed Denial of Service |
| **ECDHE** | Elliptic Curve Diffie-Hellman Ephemeral — key exchange providing forward secrecy |
| **EDR** | Endpoint Detection and Response — behavioural endpoint security with central visibility |
| **HSTS** | HTTP Strict Transport Security — forces browsers to use HTTPS |
| **MFA** | Multi-Factor Authentication — requires two or more distinct factors |
| **MITM** | Man-in-the-Middle — an attacker positioned between two communicating parties |
| **NAT** | Network Address Translation — maps private addresses to a public address |
| **OSI Model** | Seven-layer conceptual model of network communication |
| **PII** | Personally Identifiable Information |
| **RPO** | Recovery Point Objective — maximum tolerable data loss, measured in time |
| **RTO** | Recovery Time Objective — maximum tolerable downtime |
| **SIEM** | Security Information and Event Management — centralised log collection and correlation |
| **SMB** | Server Message Block — Windows file-sharing protocol (TCP 445) |
| **SPF / DKIM** | Sender Policy Framework / DomainKeys Identified Mail — email authentication mechanisms |
| **SQL Injection** | Injection of database commands through unvalidated application input |
| **STRIDE** | Threat model: Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege |
| **TCP / UDP** | Transmission Control Protocol / User Datagram Protocol — reliable vs fast transport |
| **TLS** | Transport Layer Security — encrypts and authenticates network communication |
| **VLAN** | Virtual LAN — logical network segmentation at Layer 2 |
| **WAF** | Web Application Firewall — filters Layer 7 attacks against web applications |
| **WPA2 / WPA3** | Wi-Fi Protected Access — wireless security standards |
| **XSS** | Cross-Site Scripting — injection of malicious script into a web page |
| **Zero Trust** | Security model that never implicitly trusts based on network location |

---

*End of report.*
